# Root cmake file

# version to ensure compatibility
cmake_minimum_required(VERSION 4.3)

project(query_optimizer
    VERSION 1.0.0
    DESCRIPTION "SQL Query Optimizer and execution engine"
    LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20) # C++20 specified
set(CMAKE_CXX_STANDARD_REQUIRED ON) # ensures c++20 is strictly required such that if it is on and compiler does not support c++20 then it throws error otherwise it will fall back to previous version
set(CMAKE_CXX_EXTENSION OFF) # gnu++20 to c++20, portability

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build type" FORCE)
endif()

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_COLOR_DIAGNOSTICS ON) # make warnings or error colorful

enable_testing()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# WARNINGS
if(MSVC) # microsoft visual c++
    add_compile_options(/W4 /EHsc)

# W4: warning level 4
# EHsc: Exception handling model || sc: Synchronous catch
# enables C++ exception handling (try, catch, throw)
else() # GCC, Clang or other unix-like compiler
    add_compile_options(-Wall -Wextra)

    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        add_compile_options(-g -O0)

    # -g: generate debug symbols
    # -O0: optimization level 0(none)
    else()
        add_compile_options(-O2)

        # -O2: optimization level 2
    endif()
endif()

add_executable(query_optimizer

    src/main.cpp

    # parser
    src/parser/Parser.cpp
    src/parser/AST.cpp

    # Optimizer
    src/optimizer/Optimizer.cpp
    src/optimizer/Plan.cpp

    # Executor
    src/executor/Executor.cpp
    src/executor/operators/Scan.cpp
    src/executor/operators/Filter.cpp
    src/executor/operators/Join.cpp
    src/executor/operators/Aggregate.cpp

    # Storage
    src/storage/Table.cpp
    src/storage/Tuple.cpp

    # Common
    src/common/utils.cpp
)

target_include_directories(query_optimizer PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src/parser
    ${CMAKE_CURRENT_SOURCE_DIR}/src/optimizer
    ${CMAKE_CURRENT_SOURCE_DIR}/src/executor
    ${CMAKE_CURRENT_SOURCE_DIR}/src/executor/operators
    ${CMAKE_CURRENT_SOURCE_DIR}/src/storage
    ${CMAKE_CURRENT_SOURCE_DIR}/src/common
) # tells the compiler where to look for header files

option(BUILD_TEST "Build unit tests" ON)

# ON: default value (tests are built by default)
if(BUILD_TEST)
    find_package(GTest QUIET)

    if(GTest_FOUND)
        enable_testing()
        add_subdirectory(tests)
    else()
        message(STATUS "Google Test not found. Tests disabled.")
    endif()
endif()
