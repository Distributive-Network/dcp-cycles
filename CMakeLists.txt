cmake_minimum_required(VERSION 2.8)

project(emcycles CXX)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/conf" ${CMAKE_MODULE_PATH})
include(MimicBlenderBuild)

# It was hard to make clean replacements without C++11
# add_definitions(-std=c++0x)
add_definitions(-std=c++11)

if (CMAKE_TOOLCHAIN_FILE) # lame way of detecting emscripten
  add_definitions(-Wno-warn-absolute-paths)
  add_definitions(-DEMCYCLE_JS=1)
  add_definitions(-O2)
else ()
  add_definitions(-DEMCYCLE_NORMAL=1)
endif ()

# Replacement functions
include_directories(${CMAKE_SOURCE_DIR}/src/replacements/util)
include_directories(${CMAKE_SOURCE_DIR}/cycles_hack/util)
include_directories(${CMAKE_SOURCE_DIR}/src/replacements/util)

set(BUILD_SHARED_LIBS ON)
add_definitions(-fno-exceptions)
# add_definitions(-fno-rtti)

set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
message(STATUS "Libraries are placed in ${LIBRARY_OUTPUT_PATH}")
message(STATUS "Executables are placed in ${EXECUTABLE_OUTPUT_PATH}")
make_directory(${LIBRARY_OUTPUT_PATH})
make_directory(${EXECUTABLE_OUTPUT_PATH})

add_subdirectory(cycles_hack)
