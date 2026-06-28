---
title: 如何为 Windows RT 编译 OnscripterYuri 模拟器（WIP）
categories: [教程]
author: bingtangxh
date: 2026-03-07 11:36:00
updated: 2026-03-07 11:36:00
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