# CMake Project Template
A modern C++ project template using CMake 3.25+ and [CPM.cmake](https://github.com/cpm-cmake/CPM.cmake) for dependency management, with out-of-the-box support for **Windows**, **macOS**, and **Linux**.

## Requirements

| Tool | Minimum version |
|------|----------------|
| CMake | 3.25 |
| C++ compiler | C++23-capable (MSVC 19.34+, GCC 13+, Clang 16+, AppleClang 15+) |
| Ninja *(optional)* | any recent version |

## Project Layout

```
.
├── CMakeLists.txt       # Main build script
├── CMakePresets.json    # Platform presets (Windows / macOS / Linux)
├── cmake/               # Auto-downloaded CPM.cmake lives here
└── src/
    └── main.cpp         # Application entry point
```

## Quick Start

### Using CMake Presets (recommended)

Pick the preset that matches your platform:

| Platform | Preset name |
|----------|-------------|
| Windows (Visual Studio 2022) | `windows-msvc` |
| Windows (Ninja + MSVC) | `windows-ninja` |
| Linux (GCC) | `linux-gcc` |
| Linux (Clang) | `linux-clang` |
| macOS (AppleClang) | `macos-clang` |
| Any platform – Debug | `debug` |

```bash
# Configure
cmake --preset linux-gcc        # replace with your preset

# Build
cmake --build --preset linux-gcc
```

The compiled binary is placed in `build/<preset-name>/`.

### Manual configuration

```bash
cmake -S . -B build
cmake --build build
```

## Adding Dependencies

CPM.cmake is downloaded automatically on the first CMake run.  
Add packages inside `CMakeLists.txt` after the `# Dependencies` section:

```cmake
CPMAddPackage("gh:fmtlib/fmt#11.0.2")
target_link_libraries(${PROJECT_NAME} PRIVATE fmt::fmt)
```

## Platform Notes

* **Windows** – `UNICODE`, `_UNICODE`, `WIN32_LEAN_AND_MEAN`, and `NOMINMAX` are defined automatically. MSVC warning level `/W4` is enabled.
* **macOS / Linux** – `-Wall -Wextra -Wpedantic` are passed to the compiler.
* `compile_commands.json` is always generated into the build directory for IDE/`clangd` support.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

## Acknowledgements

- [CPM.cmake](https://github.com/cpm-cmake/CPM.cmake) for dependency management

