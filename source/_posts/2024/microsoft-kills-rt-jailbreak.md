---
title: Windows RT 越狱社区和微软官方的“军备竞赛”
categories: [纪实]
author: bingtangxh
date: 2025-05-02 12:32:00
updated: 2025-05-02 12:32:00
---

## 桌面应用签名限制
# 限制描述
也许是为了在 Windows RT 设备上强推商店生态，微软从 Windows RT Build 8186 前的某个系统（Build 8061 尚无此限制）开始，禁止了不是 Microsoft Corporation 签名的桌面应用运行。试图运行时会得到提示：“Windows 无法验证此文件的数字签名。”
【这和架构不同、没转译层是两码事。架构不同你看到的是“此应用无法在你的电脑上运行”。而至于为什么 ARM （不是 ARM64）架构的桌面应用少之又少？一方面 Visual Studio 默认阻止你把桌面应用编译给 ARM 架构，会得到一句`Compiling Desktop applications for the ARM platform is not supported`。此外，用 Native C++ 编写的程序不好移植，还有那些 GNU 的应用程序也“不好搞”，似乎就只有 Qt 或者 .NET Framework 4.x 写的东西好移植。最后，外界普遍得出了“Windows RT 只能运行商店应用”的结论。】
此限制一直到 Windows RT 8.1 Build 9600 仍然存在。在 Windows 10 Version 1607 Build 15035.0 中不存在。

# 绕过的方法
首先，决定性因素是——启动测试模式。
（启动测试模式，只是只要有随便一个签名的应用可以运行了，如果完全没有签名，那还是无法运行，报错同上。我们要运行一个完全没有签名的应用程序，那就用 AnyCPU 架构并且微软签过了的 SignTool 给我们要运行的程序签一下就行了）
然而，~~你能想到微软怎么可能想不到，~~微软在 Windows RT 8.1 中（8.0 无此措施）拉黑了用于启用测试模式的启动管理器参数——`testsigning`。如果启动系统时试图让`testsigning`是 Yes ，那么系统就无法启动。接下来诞生了两种越狱方式。
- Golden Keys
原理是写了一个 Windows 启动管理器应用程序，先将自己签个名，将对应的`.p7b`证书放入信任区，然后重启电脑启动这个应用程序，然后这个应用程序修改启动策略从而解黑`testsigning`参数。
然而，好景不长，微软在 2016 年 10 月发布了更新来干死这个越狱方式。只要装了这个更新，那么每次电脑开机时都会把 Golden Keys 做的事情又给弄回去，重新拉黑`testsigning`（在微软看来，这叫“修复”）
在 Tegra 处理器的设备上，将 eMMC 所有分区都删除才能撤销更新。在 Qualcomm 处理器的设备上，那没办法，这个更新用了似乎是熔断之类的机制，无法撤销。
- Myriachan
Windows 启动管理器的检测机制负责检测有没有拉黑的参数，而实际执行时是负责实际启动的部分，或者 Windows 的内核。
而 Windows 内核有一个底层代码级别的缺陷，就是只认 ASCII 字符集的参数。
那么，Windows 启动管理器允许我们自由设置`loadoptions`，也就是随便指定启动参数。我们可以在 PowerShell （不能是命令提示符，它默认用不了 UTF-8 字符集）中，运行这么两行指令：

`bcdedit /set '{bootmgr}' loadoptions ' /TŅSTSIGNING'`
`bcdedit /set '{default}' loadoptions ' /TŅSTSIGNING'`

（不能将 TŅSTSIGNING 设为 Yes 那么弄，因为本来就没这个开关）
发现哪里不太一样了吗？就是这个 'Ņ' 。它是 U+0145 。`/TŅSTSIGNING`看起来人畜无害， Windows 启动管理器会给它放行。然而到了内核就“急转直下”了——这个字符会被截断成 ASCII 字符，截断出来的十六进制 ASCII 码就是 0x45，现在如果你不知道 0x45 对应的字符是什么，那你就猜一下。

没错，它就是——'E'！

因此，内核得到的参数就是——`/TESTSIGNING`！

内核就这么傻不拉几地以测试模式启动了，Windows 启动管理器毫不知情。~~微软看到要气死了~~

然后，这个方法还是在 2015 年 8 月被更新堵住了，不过任何设备都可以清空 eMMC 的分区来撤销更新。

- UMCI Audit Mode
这个只适用于关闭了安全启动（方法后面会说）的 Tegra 设备，不适用于 Qualcomm 设备或者没关安全启动的 Tegra 设备。其实就是改一个系统的注册表，那么系统就可以运行任何不管签没签的桌面应用程序。

- 装 Windows 10 Version 1607 Build 15035
这个不用多说了吧，装个系统的事。不过 Surface 2 需要先关闭安全启动（方法后面会说）

## Secure Boot 安全启动
# 限制描述
众所周知，大概是从 Windows 10 时代开始，预装 Windows 操作系统的设备都是启用了安全启动的（这和设备加密常常一起被提及，但实际上是彻头彻尾的两个功能），有的设备可以在 UEFI 固件设置中轻松地禁用，有的却不提供此选项。
启用了安全启动，那么这台电脑就几乎仅能启动 Windows 系统，而且过期的测试版 Windows 也不行。（一般想让电脑启动 Linux 系统或者 PhoenixOS ，相关文档都会要求你关闭安全启动）
Windows RT 设备当然也不例外。而且更“油饼”的是，以 Surface RT 为甚的设备不提供关闭安全启动的选项，甚至根本不让你进入 UEFI 固件设置。

# 绕过的方法
只有装了 Golden Keys （方法前文说了）的 Tegra 设备才能关。有人发现了一个漏洞，然后上报漏洞的同时写了两个 UEFI 应用程序，一个叫`Yahallo.efi`，一个叫`YahalloUndo.efi`~~（谁会闲到去用后者）~~用 Windows 启动管理器运行前者就可以破坏安全启动，从而彻底关闭它，同时开机还不会红屏。（我们都知道 Surface Pro 1~3 如果关闭了安全启动，开机就会红屏，但仍能进入系统）（此前有群友表示它的 Surface RT 开机红屏且无法进入 Windows RT 8.1 操作系统，说明这个的安全启动是通过一种比较合规的方式关掉了的，开机时会红屏，理论上这种设备不可能“流落民间”）
这样我们就可以启动 Android 、 Linux 等其他操作系统了。由于未知原因，在 Surface 2 上启动 Windows 10 Version 1607 Build 15035.0 也需要关闭安全启动。

## 商店应用侧载限制
# 限制描述
微软在 Windows 8 系列中默认只允许你从商店下载商店应用，不允许本地侧载安装。
以下是可能的方法：
- 侧载密钥（卖 3000 美元，而且微软还不卖了）
- 启用 LOB 侧载
用一个 AnyCPU 的 ProductPolicyEditor 在 Setup 模式下，关闭 sppsvc 服务，再将两个注册表值（直接注册表或者指令改不好改）由 0 改成 1 就行了。详见[在2024年为Windows8.x系列成功侧载第三方metro应用 复活你的Surface RT!](https://www.bilibili.com/video/BV1XS421w7uX/)
可是，后果是什么呢？
你不关闭 sppsvc ，那么 sppsvc 会贴心地将这两个数值又给改回去。而关闭 sppsvc 会使激活失效，而且 Office RT 也就无法打开了。
那如果先关闭 sppsvc ，然后装上要装的软件，最后再打开 sppsvc 呢？也不行。你装的软件会变成“无法打开这个应用”。
- 替换并重新加载 SKU 顺便获取开发者证书
大概步骤：
1. 替换系统的 SKU 相关文件，换成嵌入工业版的，并用指令重新加载相关文件
2. 指定 KMS 服务器和产品密钥，激活系统
3. 把 Office 的相关文件也换成 Pro Plus VL 的，用 KMS 激活。
完事。详细步骤可以在 Surface RT 交流群或者 Open RT Discord 获得。

装软件用的是`add-appxpackage`这个 PowerShell 指令，我们可以将这个指令弄进右键菜单和打开方式，并关联给`.appx`和`.appxbundle`这两个扩展名。
