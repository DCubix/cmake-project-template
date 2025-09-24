cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

# TODO: Set your project name
project(ExampleProject)

# Set C and C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Enable unicode
add_definitions(-DUNICODE -D_UNICODE)

# Get sources from src directory
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.c" "src/*.cc" "src/*.cxx")
file(GLOB_RECURSE HEADERS "src/*.h" "src/*.hpp" "src/*.hh" "src/*.hxx")

# Add executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Download CPM.cmake into the cmake folder
if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
    file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
endif()

# Download CPM
file(
    DOWNLOAD
    https://github.com/cpm-cmake/CPM.cmake/releases/latest/download/CPM.cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake
)
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake)

# Add CPM modules
# TODO: Add your dependencies here and link the libraries
