---
title: 如何为 Windows RT 编译 OnscripterYuri 模拟器（WIP）
categories: [教程]
author: bingtangxh
date: 2026-03-07 11:36:00
updated: 2026-11-24 18:02:00
description: 当时 CMake 和 vcpkg 快把我气死了，本文用来记录一下当时是怎么解决的。但时间久远，已经不怎么记得起来当时的细节了，而且现在我再复现一次当年的操作，却会发现操作异常顺利，所以这里真就没啥可写的。
---

# 预备条件
大概就是这些东西：
- Git
- 用来访问 GitHub 的代理
- GitHub Desktop
- Visual Studio 2022 （当然如果你的开发机是 Windows 8.1，那么 2015 也行）
- CMake 3.24.x （并添加到系统 PATH）（后续步骤会自动下载最新版本的 CMake，但是咱还是需要用到 3.24）
- Windows SDK 10.0.22621.0 （Visual Studio Installer 默认会想让你使用 10.0.26100.x ，卸掉 26100，用 22621）
- Everything

# 安装 VCPKG 包并进行 CMake 创建解决方案
- 打开 Developer Command Prompt for VS 2022，用 cd 命令切换到`vcpkg.json`所在的文件夹。
- 运行 `vcpkg install --triplet=arm-windows-static`。
- 为啥是 static ？这样可以让生成的链接库使用静态链接，最后的 exe 文件可以是单个文件，十分轻巧。最好不要在这里输入 `arm-windows`。
- 然后输入 `md build-arm`，创建 build-arm 这个文件夹。
- 输入 `cd build-arm`，进入 build-arm 文件夹。
- 输入 `cmake .. -G "Visual Studio 14 2015" -A ARM`，生成解决方案。
- （为啥要进入 build 文件夹再让 cmake 生成上一级目录？AI 是让我这么干的）
- 接着应该可以生成出来解决方案了。上面这个指令生成的是 VS2015 的解决方案，你也可以省去 `-G` 参数来得到默认 VS2022 的解决方案。
- 注意，CMake 默认只支持生成面向单一架构的解决方案，并不支持生成同时支持 x86 和 ARM 的解决方案。所以上面我设置的目录名是 `build-arm`。
- 如果需要在自己电脑上进行调试，那你还需要生成 x86 的解决方案，你应该得再新建并进入一个 `build-x86` 的目录，然后运行 `cmake .. -A x86`（默认应该就是 x86）。

# 在 Visual Studio 内
这个应该好说了。如果遇到链接器“无法打开输入文件”的错误，可以试着在项目属性里把链接器的附加库当中报错的静态库文件名删掉，再生成，也许那个 lib 文件只是写在那里，然后 SDK 里找不到，导致无法链接，而实际上又不需要那个 lib 就可以生成。比如我记得的有一个 `dinput8.lib`。