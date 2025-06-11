# Rockchip MPP 跨平台构建

[English](README_EN.md) | **中文**

## ⚠️ 重要说明

**Rockchip 官方仅发布 Android 和 ARM Linux 版本的 MPP 库。本项目中支持的其他平台版本（如 x86、Windows、macOS 等）仅用于测试和开发目的，不保证在生产环境中的稳定性。请根据您的实际需求谨慎使用。**

## 项目介绍

这是一个基于 Zig 工具链的 Rockchip MPP（Media Processing Platform）跨平台构建项目。MPP 是 Rockchip 提供的硬件加速多媒体处理平台，支持视频编解码、图像处理等功能。

本项目旨在为不同平台提供 MPP 库的编译支持，特别是在非 ARM 架构上进行开发和测试。

## 支持的平台

### 🐧 Linux 平台
- `x86_64-linux-gnu` - x86_64 Linux (GNU libc)
- `aarch64-linux-gnu` - ARM64 Linux (GNU libc)
- `arm-linux-gnueabihf` - ARM 32-bit Linux (GNU libc)
- `riscv64-linux-gnu` - RISC-V 64-bit Linux
- `loongarch64-linux-gnu` - LoongArch 64-bit Linux

### 🤖 Android 平台
- `aarch64-linux-android` - ARM64 Android
- `arm-linux-android` - ARM 32-bit Android
- `x86-linux-android` - x86 32-bit Android
- `x86_64-linux-android` - x86_64 Android

### 🌐 HarmonyOS 平台
- `aarch64-linux-harmonyos` - ARM64 HarmonyOS
- `arm-linux-harmonyos` - ARM 32-bit HarmonyOS
- `x86_64-linux-harmonyos` - x86_64 HarmonyOS

### 🪟 Windows 平台（实验性）
- `x86_64-windows-gnu` - x86_64 Windows (MinGW)
- `aarch64-windows-gnu` - ARM64 Windows (MinGW)

### 🍎 macOS 平台（实验性）
- `x86_64-macos` - x86_64 macOS
- `aarch64-macos` - ARM64 macOS (Apple Silicon)

## 快速开始

### 环境要求

- [Zig](https://ziglang.org/) 0.14.0 或更高版本
- [CMake](https://cmake.org/) 3.10 或更高版本
- Git（用于下载源码）

### 基本用法

1. **克隆仓库**
   ```bash
   git clone <your-repo-url>
   cd rkmpp
   ```

2. **构建默认平台（x86_64-linux-gnu）**
   ```bash
   ./build_with_zig.sh
   ```

3. **构建指定平台**
   ```bash
   ./build_with_zig.sh --target=aarch64-linux-gnu
   ```

4. **启用大小优化**
   ```bash
   ./build_with_zig.sh --target=x86_64-linux-gnu --optimize-size
   ```

### 构建选项

- `--target=<目标>` - 指定目标架构
- `--optimize-size` - 启用库文件大小优化
- `--help` - 显示帮助信息
- `clean` - 清除构建目录
- `clean-dist` - 清除构建和安装目录

### Android 构建

构建 Android 版本需要 Android NDK：

```bash
export ANDROID_NDK_HOME=/path/to/android-ndk
./build_with_zig.sh --target=aarch64-linux-android
```

### HarmonyOS 构建

构建 HarmonyOS 版本需要 HarmonyOS SDK：

```bash
export HARMONYOS_SDK_HOME=/path/to/harmonyos-sdk
./build_with_zig.sh --target=aarch64-linux-harmonyos
```

## 构建产物

编译成功后，产物将位于 `rkmpp_install/Release/<target>/` 目录：

```
rkmpp_install/Release/<target>/
├── include/           # 头文件
│   ├── mpp_*.h
│   ├── rk_*.h
│   └── vpu*.h
├── lib/              # 库文件
│   ├── librockchip_mpp.a    # 静态库
│   ├── librockchip_mpp.so*  # 动态库
│   └── pkgconfig/           # pkg-config文件
```

## 使用方法

### CMake 集成

```cmake
find_package(PkgConfig REQUIRED)
pkg_check_modules(MPP REQUIRED rockchip_mpp)

target_link_libraries(your_target ${MPP_LIBRARIES})
target_include_directories(your_target PRIVATE ${MPP_INCLUDE_DIRS})
target_compile_options(your_target PRIVATE ${MPP_CFLAGS_OTHER})
```

### 手动链接

```bash
gcc your_app.c -I/path/to/rkmpp_install/include -L/path/to/rkmpp_install/lib -lrockchip_mpp
```

## 自动化构建

本项目支持 GitHub Actions 自动化构建，可以同时为多个平台生成发布包。

### 手动触发构建

1. 访问 GitHub 仓库的 Actions 页面
2. 选择 "Release Build ALL Platforms" 工作流
3. 点击 "Run workflow" 并配置选项
4. 等待构建完成并下载发布包

### 支持的构建配置

- **全平台构建**: 构建所有支持的平台
- **选择性构建**: 只构建指定的平台
- **大小优化**: 生成优化过的小尺寸库文件
- **调试版本**: 包含调试信息的构建

## 注意事项

1. **平台兼容性**：非 ARM 平台的构建主要用于开发测试，实际硬件加速功能可能无法正常工作
2. **依赖管理**：某些平台可能需要额外的系统依赖或 SDK
3. **性能考虑**：跨平台编译的性能可能不如原生编译
4. **许可证**：请确保遵守 Rockchip MPP 的相关许可证条款

## 故障排除

### 常见问题

**Q: 编译失败，提示找不到头文件**
A: 检查 Zig 和 CMake 是否正确安装，确保版本符合要求

**Q: Android 构建失败**
A: 确保 `ANDROID_NDK_HOME` 环境变量设置正确，并且 NDK 版本兼容

**Q: 库文件无法链接**
A: 检查目标平台是否一致，确保使用相同的工具链进行编译和链接

### 获取帮助

如果遇到问题，可以：

1. 查看构建日志中的详细错误信息
2. 在 GitHub 仓库中创建 Issue
3. 参考 `.github/scripts/BUILD_GUIDE.md` 文档

## 贡献

欢迎提交 Pull Request 来改进这个项目！请确保：

1. 新增的功能有相应的文档说明
2. 通过了基本的构建测试
3. 遵循项目的代码风格

## 许可证

本项目遵循相应的开源许可证。使用前请仔细阅读许可证条款。

---

**免责声明**：本项目非 Rockchip 官方项目，仅用于学习和开发目的。使用本项目产生的任何问题，开发者不承担责任。
