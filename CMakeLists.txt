# Root cmake file

# version to ensure compatibility
cmake_minimum_required(VERSION 3.20)

project(SQLQueryOptimizer
    VERSION 1.0.0
    DESCRIPTION "SQL Query Optimizer and execution engine"
    LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20) # C++20 specified
set(CMAKE_CXX_STANDARD_REQUIRED ON) # ensures c++20 is strictly required such that if it is on and compiler does not support c++20 then it throws error otherwise it will fall back to previous version
set(CMAKE_CXX_EXTENSION OFF) # gnu++20 to c++20, portability

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_COLOR_DIAGNOSTICS ON) # make warnings or error colorful

enable_testing()

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

include(cmake/compiler_flags.cmake) # Module

include(FetchContent)

FetchContent_Declare(
    antlr4
    GIT_REPOSITORY
)