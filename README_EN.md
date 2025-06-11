# Rockchip MPP Cross-Platform Build

**English** | [中文](README.md)

## ⚠️ Important Notice

**Rockchip officially only releases MPP libraries for Android and ARM Linux platforms. The other platform versions supported in this project (such as x86, Windows, macOS, etc.) are for testing and development purposes only, and stability in production environments is not guaranteed. Please use them cautiously according to your actual needs.**

## Project Overview

This is a cross-platform build project for Rockchip MPP (Media Processing Platform) based on the Zig toolchain. MPP is a hardware-accelerated multimedia processing platform provided by Rockchip, supporting video codec, image processing, and other functions.

This project aims to provide compilation support for MPP libraries on different platforms, especially for development and testing on non-ARM architectures.

## Supported Platforms

### 🐧 Linux Platforms
- `x86_64-linux-gnu` - x86_64 Linux (GNU libc)
- `aarch64-linux-gnu` - ARM64 Linux (GNU libc)
- `arm-linux-gnueabihf` - ARM 32-bit Linux (GNU libc)
- `riscv64-linux-gnu` - RISC-V 64-bit Linux
- `loongarch64-linux-gnu` - LoongArch 64-bit Linux

### 🤖 Android Platforms
- `aarch64-linux-android` - ARM64 Android
- `arm-linux-android` - ARM 32-bit Android
- `x86-linux-android` - x86 32-bit Android
- `x86_64-linux-android` - x86_64 Android

### 🌐 HarmonyOS Platforms
- `aarch64-linux-harmonyos` - ARM64 HarmonyOS
- `arm-linux-harmonyos` - ARM 32-bit HarmonyOS
- `x86_64-linux-harmonyos` - x86_64 HarmonyOS

### 🪟 Windows Platforms (Experimental)
- `x86_64-windows-gnu` - x86_64 Windows (MinGW)
- `aarch64-windows-gnu` - ARM64 Windows (MinGW)

### 🍎 macOS Platforms (Experimental)
- `x86_64-macos` - x86_64 macOS
- `aarch64-macos` - ARM64 macOS (Apple Silicon)

## Quick Start

### Prerequisites

- [Zig](https://ziglang.org/) 0.14.0 or higher
- [CMake](https://cmake.org/) 3.10 or higher
- Git (for downloading source code)

### Basic Usage

1. **Clone Repository**
   ```bash
   git clone <your-repo-url>
   cd rkmpp
   ```

2. **Build Default Platform (x86_64-linux-gnu)**
   ```bash
   ./build_with_zig.sh
   ```

3. **Build Specific Platform**
   ```bash
   ./build_with_zig.sh --target=aarch64-linux-gnu
   ```

4. **Enable Size Optimization**
   ```bash
   ./build_with_zig.sh --target=x86_64-linux-gnu --optimize-size
   ```

### Build Options

- `--target=<target>` - Specify target architecture
- `--optimize-size` - Enable library size optimization
- `--help` - Show help information
- `clean` - Clean build directory
- `clean-dist` - Clean build and install directories

### Android Build

Building for Android requires Android NDK:

```bash
export ANDROID_NDK_HOME=/path/to/android-ndk
./build_with_zig.sh --target=aarch64-linux-android
```

### HarmonyOS Build

Building for HarmonyOS requires HarmonyOS SDK:

```bash
export HARMONYOS_SDK_HOME=/path/to/harmonyos-sdk
./build_with_zig.sh --target=aarch64-linux-harmonyos
```

## Build Output

After successful compilation, the artifacts will be located in `rkmpp_install/Release/<target>/`:

```
rkmpp_install/Release/<target>/
├── include/           # Header files
│   ├── mpp_*.h
│   ├── rk_*.h
│   └── vpu*.h
├── lib/              # Library files
│   ├── librockchip_mpp.a    # Static library
│   ├── librockchip_mpp.so*  # Shared library
│   └── pkgconfig/           # pkg-config files
```

## Usage

### CMake Integration

```cmake
find_package(PkgConfig REQUIRED)
pkg_check_modules(MPP REQUIRED rockchip_mpp)

target_link_libraries(your_target ${MPP_LIBRARIES})
target_include_directories(your_target PRIVATE ${MPP_INCLUDE_DIRS})
target_compile_options(your_target PRIVATE ${MPP_CFLAGS_OTHER})
```

### Manual Linking

```bash
gcc your_app.c -I/path/to/rkmpp_install/include -L/path/to/rkmpp_install/lib -lrockchip_mpp
```

## Automated Builds

This project supports GitHub Actions automated builds, which can generate release packages for multiple platforms simultaneously.

### Manual Build Trigger

1. Visit the Actions page of the GitHub repository
2. Select "Release Build ALL Platforms" workflow
3. Click "Run workflow" and configure options
4. Wait for build completion and download release packages

### Supported Build Configurations

- **All Platform Build**: Build all supported platforms
- **Selective Build**: Build only specified platforms
- **Size Optimization**: Generate optimized small-size libraries
- **Debug Build**: Build with debug information

## Important Notes

1. **Platform Compatibility**: Builds for non-ARM platforms are mainly for development testing, actual hardware acceleration features may not work properly
2. **Dependency Management**: Some platforms may require additional system dependencies or SDKs
3. **Performance Considerations**: Cross-platform compilation performance may not be as good as native compilation
4. **Licensing**: Please ensure compliance with relevant Rockchip MPP license terms

## Troubleshooting

### Common Issues

**Q: Build fails with header file not found**
A: Check if Zig and CMake are installed correctly and meet version requirements

**Q: Android build fails**
A: Ensure `ANDROID_NDK_HOME` environment variable is set correctly and NDK version is compatible

**Q: Library files cannot be linked**
A: Check if target platforms are consistent and use the same toolchain for compilation and linking

### Getting Help

If you encounter problems, you can:

1. Check detailed error information in build logs
2. Create an Issue in the GitHub repository
3. Refer to `.github/scripts/BUILD_GUIDE.md` documentation

## Contributing

Pull Requests are welcome to improve this project! Please ensure:

1. New features have corresponding documentation
2. Pass basic build tests
3. Follow project code style

## License

This project follows the corresponding open source license. Please read the license terms carefully before use.

---

**Disclaimer**: This project is not an official Rockchip project and is for learning and development purposes only. The developers are not responsible for any issues arising from the use of this project.
