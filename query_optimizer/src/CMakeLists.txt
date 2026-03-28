cmake_minimum_required(VERSION 3.20)

project(SQLQueryOptimizer VERSION 1.0.0
    DESCRIPTION "SQL Query Optimizer and Execution Engine"
    LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(CTest)
enable_testing()

add_executable(MyCMakeproject engine.cpp)

set_property(TARGET MyCMakeproject PROPERTY CXX_STANDARD 20)
