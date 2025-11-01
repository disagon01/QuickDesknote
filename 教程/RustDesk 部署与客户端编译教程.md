# RustDesk 客户端编译教程

# 1、客户端编译环境部署

### 2.1 基础编辑工具

* **VS Code（推荐）**：[https://code.visualstudio.com/](https://code.visualstudio.com/)

  用于编辑源代码，也可替换为其他文本编辑软件。
* **Git 管理工具**：[https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)

  安装时选择 “一路下一步”，默认配置即可。

### 2.2 编译依赖工具

#### 2.2.1 Microsoft C++ 生成工具

1. 下载地址：[https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)
2. 安装时必须勾选以下组件：

* 使用 C++ 的桌面开发
* Windows10 SDK
* 对 v143 生成工具 (最新) 的 C++/CLI 支持
* 适用于 v143 生成工具的 C++ 模块 (x64/x86)
* 适用于 Windows 的 C++ Clang 工具

#### 2.2.2 Rust 语言环境

1. 下载地址：[https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)
2. 运行安装程序后，输入 “1” 并回车，执行默认安装。

#### 2.2.3 vcpkg 包管理工具（CMD 命令）

1. 切换到 C 盘根目录：

```
cd c:\\
```

1. 克隆 vcpkg 仓库：

```
git clone https://github.com/microsoft/vcpkg
```

1. 进入 vcpkg 目录并初始化：

```
cd vcpkg

bootstrap-vcpkg.bat
```

1. 安装依赖包（均为 x64 静态编译版本）：

```
vcpkg install libvpx:x64-windows-static

vcpkg install aom:x64-windows-static

vcpkg install opus:x64-windows-static

vcpkg install libyuv:x64-windows-static
```

1. 配置环境变量：

```
setx VCPKG\_ROOT C:\vcpkg
```

#### 2.2.4 FlutterUI 框架（CMD 命令）

1. 切换到 C 盘根目录：

```
cd c:\\
```

1. 克隆 Flutter 仓库（指定 3.27.2 版本）：

```
git clone https://github.com/flutter/flutter.git -b 3.27.2
```

1. 配置 Flutter 环境变量：

```
setx PATH "%PATH%;C:\flutter\bin"
```

1. 安装 Flutter-Rust 桥接工具：

```
cargo install flutter\_rust\_bridge\_codegen --version 1.80.1 --features uuid --locked
```

#### 2.2.5 其他工具

* **LLVM 编译工具**：[https://github.com/llvm/llvm-project/releases/download/llvmorg-20.1.7/LLVM-20.1.7-win64.exe](https://github.com/llvm/llvm-project/releases/download/llvmorg-20.1.7/LLVM-20.1.7-win64.exe)

  安装时选择 “第二个选项”（默认安装路径即可）。
* **Python 3.13.5**：[https://www.python.org/downloads/](https://www.python.org/downloads/)

  安装时必须勾选 “Add python.exe to PATH”，确保命令行可调用 Python。

### 2.3 源码准备与编译

#### 2.3.1 下载源码

需下载两个仓库的源码：

1. RustDesk 主源码：[https://github.com/rustdesk/rustdesk](https://github.com/rustdesk/rustdesk)
2. hbb\_common 依赖源码：[https://github.com/rustdesk/hbb\_common](https://github.com/rustdesk/hbb_common)
3. 解压配置：将 hbb\_common 解压到 `*:\RustDesk\libs\` 目录下（“\*” 为 RustDesk 主源码的解压盘符）。

#### 2.3.2 生成桥接文件（CMD 命令）

1. 切换到 RustDesk 主源码目录：

```
cd "RustDesk目录"
```

1. 执行桥接文件生成命令：

```
flutter\_rust\_bridge\_codegen --rust-input ./src/flutter\_ffi.rs --dart-output ./flutter/lib/generated\_bridge.dart --c-output ./flutter/macos/Runner/bridge\_generated.h
```

#### 2.3.3 编译客户端（CMD 命令）

在 RustDesk 主源码目录下执行：

```
python build.py --portable --flutter --skip-portable-pack
```
