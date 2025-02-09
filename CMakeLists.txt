cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

# TODO: Set your project name
project(ExampleProject)

set(BUILD_DIR ${CMAKE_CURRENT_SOURCE_DIR}/build)

# Setup build directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${BUILD_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${BUILD_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${BUILD_DIR})

# MSVC makes Debug and Release directories, handle them
if (MSVC)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${BUILD_DIR}/Debug)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${BUILD_DIR}/Release)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG ${BUILD_DIR}/Debug)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE ${BUILD_DIR}/Release)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${BUILD_DIR}/Debug)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${BUILD_DIR}/Release)
endif()

# Set MSVC C and C++ standard
if (MSVC)
    add_compile_options(/std:c++latest)
    add_compile_options(/std:c11)
else()
    set(CMAKE_CXX_STANDARD 21)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
    set(CMAKE_C_STANDARD 11)
    set(CMAKE_C_STANDARD_REQUIRED ON)
    set(CMAKE_C_EXTENSIONS OFF)
endif()

# enable unicode
add_definitions(-DUNICODE -D_UNICODE)

# Get sources from src directory
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.c" "src/*.cc" "src/*.cxx")
file(GLOB_RECURSE HEADERS "src/*.h")

# Add executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Download CPM.cmake into the cmake folder
if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
    file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
endif()

# Download CPM
# TODO: Get the latest version from: https://github.com/cpm-cmake/CPM.cmake/releases/latest
file(
    DOWNLOAD
    https://github.com/cpm-cmake/CPM.cmake/releases/download/v0.40.5/CPM.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake
)
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake)

# Add CPM modules
# TODO: Add your dependencies here and link the libraries
