# Rockchip MPP 自动构建系统

这个GitHub Actions工作流可以为多个平台自动构建Rockchip MPP库。

## 功能特性

- 🚀 支持多平台交叉编译（Linux、Windows、macOS、Android、HarmonyOS）
- 🔧 使用Zig工具链进行跨平台构建
- 📦 自动打包和发布
- ✅ 构建验证和测试
- 🏷️ 灵活的版本标签和目标选择

## 支持的平台

### Linux
- `x86_64-linux-gnu` - x86_64 Linux (GNU libc)
- `aarch64-linux-gnu` - ARM64 Linux (GNU libc)  
- `arm-linux-gnueabihf` - ARM 32-bit Linux (GNU libc)
- `riscv64-linux-gnu` - RISC-V 64-bit Linux
- `loongarch64-linux-gnu` - LoongArch 64-bit Linux

### Windows
- `x86_64-windows-gnu` - x86_64 Windows (MinGW)
- `aarch64-windows-gnu` - ARM64 Windows (MinGW)

### macOS
- `x86_64-macos` - x86_64 macOS
- `aarch64-macos` - ARM64 macOS (Apple Silicon)

### Android
- `aarch64-linux-android` - ARM64 Android
- `arm-linux-android` - ARM 32-bit Android
- `x86-linux-android` - x86 32-bit Android
- `x86_64-linux-android` - x86_64 Android

### HarmonyOS
- `aarch64-linux-harmonyos` - ARM64 HarmonyOS
- `arm-linux-harmonyos` - ARM 32-bit HarmonyOS
- `x86_64-linux-harmonyos` - x86_64 HarmonyOS

## 如何使用

### 1. 手动触发构建

1. 在GitHub仓库页面，点击 "Actions" 标签
2. 选择 "Release Build ALL Platforms" 工作流
3. 点击 "Run workflow" 按钮
4. 配置构建选项：
   - **Release version tag**: 发布版本标签（如：v1.0.0）
   - **Build targets**: 构建目标（"all" 或逗号分隔的目标列表）
   - **Enable debug build**: 是否启用调试构建

### 2. 构建选项示例

#### 构建所有平台
```
Release version tag: v1.0.0
Build targets: all
Enable debug build: false
```

#### 只构建Linux平台
```
Release version tag: v1.0.1
Build targets: x86_64-linux-gnu,aarch64-linux-gnu
Enable debug build: false
```

#### 构建Android平台（调试版本）
```
Release version tag: v1.0.0-debug
Build targets: aarch64-linux-android,arm-linux-android
Enable debug build: true
```

### 3. 构建产物

构建完成后，将会创建一个GitHub Release，包含：

- 每个平台的压缩包（.tar.gz）
- 构建信息和校验和
- 发布说明

## 文件结构

```
.github/
├── workflows/
│   └── Release.yml           # 主要的GitHub Actions工作流
└── scripts/
    ├── validate_build.sh     # 构建验证脚本
    └── BUILD_GUIDE.md       # 本文档
```

## 构建产物结构

每个构建包含以下内容：

```
rockchip-mpp-{version}-{target}/
├── include/                 # 头文件
│   ├── mpp_*.h
│   ├── rk_*.h
│   └── vpu*.h
├── lib/                     # 库文件
│   ├── librockchip_mpp.a    # 静态库
│   ├── librockchip_mpp.so*  # 动态库（Linux）
│   └── pkgconfig/           # pkg-config文件
├── BUILD_INFO.txt           # 构建信息
└── CHECKSUMS.txt           # 文件校验和
```

## 环境要求

### GitHub Actions
- Ubuntu 22.04, Windows 2022, macOS 13
- Zig 0.14.0
- CMake
- 各平台对应的SDK（Android NDK、HarmonyOS SDK等）

### 本地构建
- Zig 0.14.0
- CMake 3.10+
- 支持的编译器

## 故障排除

### 常见问题

1. **构建失败**
   - 检查构建日志中的错误信息
   - 确认目标平台是否受支持
   - 验证环境依赖是否正确安装

2. **找不到库文件**
   - 检查 `validate_build.sh` 的输出
   - 确认构建过程是否完成
   - 检查CMake配置是否正确

3. **压缩包损坏**
   - 检查构建产物的完整性
   - 使用校验和验证文件

### 调试技巧

1. **启用调试模式**
   - 在触发工作流时启用 "Enable debug build"
   - 查看详细的构建日志

2. **检查依赖**
   - 确认所有必要的工具已安装
   - 验证环境变量设置

## 贡献

如果您需要添加新的目标平台或改进构建过程：

1. 修改 `.github/workflows/Release.yml`
2. 更新 `build_with_zig.sh` 脚本
3. 添加相应的验证逻辑到 `validate_build.sh`
4. 测试您的更改
5. 提交Pull Request

## 许可证

本构建系统遵循项目的许可证条款。

## 联系方式

如有问题，请在GitHub仓库中创建Issue。
