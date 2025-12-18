cmake_minimum_required(VERSION 3.15)

project(zarchiver)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(zarchiver main.cpp)

target_link_libraries(zarchiver PRIVATE)