cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# TODO: Set your project name and version
project(ExampleProject
    VERSION 1.0.0
    LANGUAGES C CXX
)

# Prevent in-source builds
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
    message(FATAL_ERROR
        "In-source builds are not allowed. "
        "Please create a separate build directory and run CMake from there."
    )
endif()

# Default to Debug when no build type is specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build type" FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
        Debug Release MinSizeRel RelWithDebInfo
    )
endif()

# Export compile_commands.json for IDE / clangd support
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# ── Language standards ─────────────────────────────────────────────────────────
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# ── Sources ────────────────────────────────────────────────────────────────────
# CONFIGURE_DEPENDS re-runs CMake when files are added/removed
file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS "src/*.cpp" "src/*.c" "src/*.cc" "src/*.cxx")
file(GLOB_RECURSE HEADERS CONFIGURE_DEPENDS "src/*.h" "src/*.hpp" "src/*.hh" "src/*.hxx")

add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Enforce the C++ standard at target scope (supplements the global defaults above)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_23)

# ── Platform-specific settings ─────────────────────────────────────────────────
if(WIN32)
    # Enable Unicode and trim the Windows header set
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        UNICODE
        _UNICODE
        WIN32_LEAN_AND_MEAN
        NOMINMAX
    )
    # MSVC warning level
    if(MSVC)
        target_compile_options(${PROJECT_NAME} PRIVATE /W4)
    endif()
elseif(APPLE OR UNIX)
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
endif()

# ── CPM.cmake ──────────────────────────────────────────────────────────────────
set(CPM_DOWNLOAD_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake")

message(STATUS "Downloading latest CPM.cmake...")
file(MAKE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
file(
    DOWNLOAD
    "https://github.com/cpm-cmake/CPM.cmake/releases/latest/download/CPM.cmake"
    "${CPM_DOWNLOAD_LOCATION}"
)

include("${CPM_DOWNLOAD_LOCATION}")

# ── Dependencies ───────────────────────────────────────────────────────────────
# TODO: Add your CPM dependencies here and link the libraries, e.g.:
# CPMAddPackage("gh:fmtlib/fmt#11.0.2")
# target_link_libraries(${PROJECT_NAME} PRIVATE fmt::fmt)
