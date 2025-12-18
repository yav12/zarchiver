cmake_minimum_required(VERSION 3.15)

project(zarchiver)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# do the submodule stuff
find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${CMAKE_SOURCE_DIR}/.git")
    message(STATUS "Updating git submodules")
    execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                    RESULT_VARIABLE GIT_SUBMOD_RESULT)
    if(NOT GIT_SUBMOD_RESULT EQUAL "0")
        message(FATAL_ERROR "Git submodule update failed with ${GIT_SUBMOD_RESULT}")
    endif()
endif()

#initialize vcpkg if not already done
execute_process(COMMAND ${CMAKE_SOURCE_DIR}/vcpkg/bootstrap-vcpkg.sh
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/vcpkg
                RESULT_VARIABLE VCPKG_BOOTSTRAP_RESULT)
if(NOT VCPKG_BOOTSTRAP_RESULT EQUAL "0")
    message(FATAL_ERROR "Vcpkg bootstrap failed with ${VCPKG_BOOTSTRAP_RESULT}")
endif()

#set the toolchain file for vcpkg
if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "${CMAKE_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake"
        CACHE STRING "")
endif()

find_package(PcapPlusPlus REQUIRED)

add_executable(zarchiver main.cpp)

target_link_libraries(zarchiver PRIVATE Pcap++ Packet++ Common++ pcap pthread)