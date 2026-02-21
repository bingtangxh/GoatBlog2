---
title: Windows RT 越狱社区和微软官方的"军备竞赛"
categories: [纪实]
author: bingtangxh
date: 2025-05-02 12:32:00
updated: 2025-07-09 08:45:00
---

## 桌面应用签名限制

### 限制描述

也许是为了在 Windows RT 设备上强推商店生态，微软从 Windows RT Build 8186 前的某个系统（Build 8061 尚无此限制）开始，禁止了不是 Microsoft Corporation 签名的桌面应用运行。试图运行时会得到提示："Windows 无法验证此文件的数字签名。"，而如果签名已经签了，但是却不是 Microsoft Corporation 签名，或者桌面应用程序不是 ARM 架构的（绝大多数的桌面应用都是 x86/x64 架构的），那么显示的是"此应用无法在你的电脑上运行"。

> 这和架构不同、没转译层是两码事。而至于为什么 ARM（不是 ARM64）架构的桌面应用少之又少？
>
> 1. Windows SDK 默认阻止你把桌面应用编译给 ARM 架构，会得到一句 `Compiling Desktop applications for the ARM platform is not supported`
> 2. GNU 的应用程序也"不好搞"（这个是听说的）
> 3. 我也不知道为什么，单纯因为没人用，所以开发者懒得适配测试，反正也没必要花钱买没用的测试机？
>
> 似乎就只有 Qt 或者 .NET Framework 4.x 写的东西好移植。最后，外界普遍得出了"Windows RT 只能运行商店应用"的结论。

此限制一直到 Windows RT 8.1 Build 9600 仍然存在。在 Windows 10 Version 1607 Build 15035.0 中不存在。

### 绕过的方法

首先，决定性因素是——让它不限制签名。
我们有两种方法，最开始人们使用笨办法——开启测试模式。

注意，如果你用的是 Surface 或者联想的 RT 平板，那么可以不看测试模式，直接想办法关闭安全启动。
下面四个越狱方式的前两种，即使越狱了，你的 exe 文件还是要签一下名才能运行。
注意，是“信任任何签名”，也就是随便一个签名就可以，而完全没有签名还是不行。

而咱们一般下载的 exe 都是没有签名的，即使有个 XDA 大佬写了个 `SignTool` 用来简单快速地签名（不是 Windows SDK 里面的 SignTool），那不还是多了一个步骤吗？

{% folding 关于那个 SignTool %}

原发布贴在这里[Windows RT 8.1 - Jailbreak Sign Tool](https://xdaforums.com/t/windows-rt-8-1-jailbreak-sign-tool.3228929/)

注意这个 SignTool 是用 .NET Framework 4.x C# 语言写的，所以同一个文件在 x86/ARM32 上都能运行。
你可以将这个工具下载下来，复制一份，用复制出来的去“签一下”原来的那个“它自己”，
然后这个签过了的 SignTool 就在 RT 上也能运行了，你下载了什么东西，用 RT 平板自己运行 SignTool 直接签，签了直接运行，
不需要另找 x86/x64/ARM64 电脑签了名然后再用 U 盘或者 SMB 共享过去，真繁琐。

{% endfolding %}

所以 Surface 或者联想的可以直接看下面的第 3~4 点，用那两个方式之后，ARM32 架构适配的 exe 文件就可以随便运行，不需要事先签名了。

但是 Qualcomm 设备（如 Lumia 2520）还是只能用开启测试模式的方法，所有 exe 文件得先签名才能运行，关不掉安全启动。

> 启动测试模式，只是只要有随便一个签名的应用可以运行了。如果完全没有签名，那还是无法运行，报错同上。我们要运行一个完全没有签名的应用程序，那就用 AnyCPU 架构并且微软签过了的 SignTool 给我们要运行的程序签一下就行了。

然而，~~你能想到微软怎么可能想不到，~~微软在 Windows RT 8.1 中（8.0 无此措施）拉黑了用于启用测试模式的启动管理器参数——`testsigning`。
首先如果你在系统里试图将 `testsigning` 设置为 Yes，会得到一句 “该值受安全引导策略保护”。
而如果你用其他方法，让 `testsigning` 最终还是变成了 Yes，那么系统就无法启动。

1. **Myriachan**  
   Windows 启动管理器的检测机制负责检测有没有拉黑的参数，而实际执行时是负责实际启动的部分，或者 Windows 的内核。  
   而 Windows 内核有一个底层代码级别的缺陷，就是只认 ASCII 字符集的参数。  
   那么，Windows 启动管理器允许我们自由设置 `loadoptions`，也就是随便指定启动参数。我们可以在 PowerShell（不能是命令提示符，它默认用不了 UTF-8 字符集）中，运行这么两行指令：

   ```powershell
   bcdedit /set '{bootmgr}' loadoptions ' /TŅSTSIGNING'
   bcdedit /set '{default}' loadoptions ' /TŅSTSIGNING'
   ```

   （不能将 TŅSTSIGNING 设为 Yes 那么弄，因为本来就没这个开关）  
   发现哪里不太一样了吗？就是这个 'Ņ'。它是 U+0145。`/TŅSTSIGNING` 看起来人畜无害，Windows 启动管理器会给它放行。然而到了内核就"急转直下"了——这个字符会被截断成 ASCII 字符，截断出来的十六进制 ASCII 码就是 0x45。
   > 现在如果你不知道 0x45 对应的字符是什么，那你就猜一下。  
   
   没错，它就是——'E'！  
   
   因此，内核得到的参数就是——`/TESTSIGNING`！  
   
   内核就这么傻不拉几地以测试模式启动了，Windows 启动管理器毫不知情。~~微软看到要气死了~~  
   
   然后，这个方法在 2015 年 8 月被微软用更新给干死了，不过任何设备都可以清空 eMMC 的分区来撤销更新。

2. **Golden Keys**  
   原理是写了一个 Windows 启动管理器应用程序，先将自己签个名，将对应的 `.p7b` 证书放入信任区，然后重启电脑启动这个应用程序，这个应用程序修改启动策略从而解黑 `testsigning` 参数。  
   然而一样的，好景不长，微软在 2016 年 10 月发布了更新来干死这个越狱方式。只要装了这个更新，那么每次电脑开机时都会把 Golden Keys 做的事情又给弄回去，重新拉黑 `testsigning`（在微软看来，这叫"修复"）。  
   在 Tegra 处理器的设备上，将 eMMC 所有分区都删除才能撤销更新。在 Qualcomm 处理器的设备上，那没办法，这个更新用了似乎是熔断之类的机制，无法撤销。
   > Golden Keys 有两个用处，一个是解黑 `testsigning` 参数，
   这样就可以直接 `bcdedit /set {current} testsigning on` 这样开启测试模式了。同时你还就可以运行 Yahallo 了。

3. **UMCI Audit Mode**  
   这个只适用于关闭了安全启动（方法后面会说）的 Tegra 设备，不适用于 Qualcomm 设备或者没关安全启动的 Tegra 设备。
   开启这个模式之后，就不需要开启测试模式了。
   其实就是改一个系统的注册表，那么系统就可以运行任何不管签没签的桌面应用程序。

4. **装 Windows 10 Version 1607 Build 15035**  
   这个不用多说了吧，装个系统的事。这个系统没有任何对 exe 的签名限制，所以只要架构是对的就可以运行。
   但是注意，启动 Windows 10 Version 1607 Build 15035 之前，你的平板必须卸掉了 Golden Keys，否则就无法启动，开机黑屏。

{% folding Surface 2 的条件有些苛刻 %}

Surface 2 如果想启动 Windows 10 Version 1607 Build 15035，首先得关闭安全启动。
而如果想关闭安全启动，需要运行 Yahallo
如果想要运行 Yahallo，需要先安装 Golden Keys
但是想要启动 Windows 10 Version 1607 Build 15035，又得先卸掉 Golden Keys

看上去像是不可行的闭环？

留意一下，只要运行了 Yahallo，破坏了安全启动，那么你接着再卸载掉 Golden Keys 的话，不会导致安全启动变回去
所以我们得三步连着来：安装 Golden Keys→安装 Yahallo→卸载 Golden Keys。

{% endfolding %}

## Secure Boot 安全启动

### 限制描述

众所周知，大概是从 Windows 10 时代开始，预装 Windows 操作系统的设备都是启用了安全启动的（这和设备加密常常一起被提及，但实际上是彻头彻尾的两个功能），有的设备可以在 UEFI 固件设置中轻松地禁用，有的却不提供此选项。  
启用了安全启动，那么这台电脑就几乎仅能启动 Windows 系统，而且过期的测试版 Windows 也不行。（一般想让电脑启动 Linux 系统或者 PhoenixOS，相关文档都会要求你关闭安全启动）  
Windows RT 设备当然也不例外。而且更"油饼"的是，以 Surface RT 为甚的设备不提供关闭安全启动的选项，甚至根本不让你进入 UEFI 固件设置。

### 绕过的方法

只有装了 Golden Keys（方法前文说了）的 Tegra 设备（Surface 或联想）才能关。NekomimiRouter 在约 2020 年发现了一个漏洞，然后上报漏洞的同时写了两个 UEFI 应用程序，一个叫 `Yahallo.efi`，一个叫 `YahalloUndo.efi`~~（谁会闲到去用后者）~~用 Windows 启动管理器运行前者就可以破坏安全启动，从而彻底关闭它，同时开机还不会红屏。

> 我们都知道 Surface Pro 1~3 如果关闭了安全启动，开机就会红屏，但仍能进入系统。（此前有群友表示它的 Surface RT 开机红屏且无法进入 Windows RT 8.1 操作系统，说明这个的安全启动是通过一种比较合规的方式关掉了的，开机时会红屏，理论上这种设备不可能"流落民间"。通过这台设备的序列号在微软支持网站上下载恢复镜像，推测这个机器是北欧地区发售的）

这样我们就可以启动 Android、Linux 等其他操作系统了。由于未知原因，在 Surface 2 上启动 Windows 10 Version 1607 Build 15035.0 也需要关闭安全启动。

> 但是任何设备启动 Windows 10 Version 1607 Build 15035.0 都需要先卸掉 Golden Keys。所以如果既想要关闭安全启动（用来启动 Linux），又想用 RT10 系统，就得三步走：安装 Golden Keys→安装 Yahallo→卸载 Golden Keys。

有人可能会默认“以为错”：
只要运行了 Yahallo，破坏了安全启动，那么你接着再卸载掉 Golden Keys 的话，不会导致安全启动变回去。
——上面这句是对的。

~~这个漏洞上报之后，微软说是因为相关产品生命周期已结束，懒得修复~~

## 商店应用侧载限制

### 限制描述

微软在 Windows 8 系列中默认只允许你从商店下载商店应用，不允许本地侧载安装。现在商店停服了也就没法下软件了。

### 绕过的方法

以下是可能的方法：

- ~~侧载密钥（卖 3000 美元，而且微软还不卖了）~~
- **启用 LOB 侧载（需要先开启测试模式，已过时不推荐）**  
  用一个 AnyCPU 的 ProductPolicyEditor （也是 C# 写的）在 Setup 模式下，关闭 sppsvc 服务，再将两个注册表值（直接注册表或者指令改不好改）由 0 改成 1 就行了。详见[在2024年为Windows8.x系列成功侧载第三方metro应用 复活你的Surface RT!](https://www.bilibili.com/video/BV1XS421w7uX/)  
  可是，后果是什么呢？  
  你不关闭 sppsvc，那么 sppsvc 会贴心地将这两个数值又给改回去。而关闭 sppsvc 会使激活失效，而且 Office RT 也就无法打开了。  
  那如果先关闭 sppsvc，然后装上要装的软件，最后再打开 sppsvc 呢？也不行。你装的软件会变成"无法打开这个应用"。
  
- **替换并重新加载 SKU 顺便获取开发者证书**  
  大概步骤：
  1. 替换系统的 SKU 相关文件，换成嵌入工业版的，并用指令重新加载相关文件
  2. 指定 KMS 服务器和产品密钥，激活系统
  3. 把 Office 的相关文件也换成 Pro Plus VL 的，用 KMS 激活。
  
  完事。详细步骤可以在 [Surface RT 交流群](http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=Ce1yu4QLXrUIFKjBznFHUly0WIMGQsyd&authKey=d9lUQrNVGTNKd2%2Bku1mIWhXFnmNUS%2Babm4vb3PzRMVzOw5Obg8Sqem0idUYxyUMe&noverify=0&group_code=725688821)或者 [Open RT Discord](https://discord.gg/VW75GmWa95) 获得。

  装软件用的是 `add-appxpackage` 这个 PowerShell 指令，我们可以将这个指令弄进右键菜单和打开方式，并关联给 `.appx` 和 `.appxbundle` 这两个扩展名。

{% folding 其实还有 Windows 8 Appx 应用的一系列限制 %}

- Appx 应用首先要有签名，以防损坏或篡改
- 签名还要是受信任的，才能安装（WP8.1 是特例无此限制，但是其他限制一个不落）
- 上传到微软商店的软件包会被微软签名。微软签名肯定是受信任的
- 你自己签的名肯定默认是不信任的，要添加信任
- 但是！微软签名的软件包，必须是从商店安装的，才能打开运行，如果微软签名的包是侧载安装上去的，那虽然能安装，但是打不开，报错“无法打开这个应用”。
- 所以，你想要安装一个 Appx 包，首先需要“重新打包签名”（可以用以前的 WSAppBak 或者后来的 Resign Tool），让它变成一个“其他签名”
- 然后再将这个签名导入信任
- （WP8.1 不需要上一步，电脑的 Win8/Win8.1/Win10/Win11 或者手机的 Win10Mobile 则都需要，具体的有点没法长话短说，就是将 cer 或 pfx 文件，或者就是 Appx 文件的数字签名，导入到本地计算机的“受信任的根证书颁发机构”里面去。如果不添加信任，安装的时候就会报错“已处理证书链，但在不受信任的根证书中终止”）
- 才能安装这个软件包并安装使用
- 没错，就是这么繁琐！要不然你就买下微软解决一切！
- 所以，如果是微软签名的包，看似不用导入证书（添加信任）就可以安装，但是安装后却无法打开，报错“无法打开这个应用”。这时就必须先卸载那个打不开的软件包，再重新打包签名，重试安装。要不然，你重新打包签名好了，还是装不上，因为已经“装过了”。
- 此外，因为没有 ARM32 的 makecert.exe （倒是有 Windows SDK 里面给 ARM32 带的 SignTool，也有泄露的 makeappx 和 makepri），所以重新签名打包的这个过程还不能在 RT 平板自身上完成，只能另找 x86/x64/ARM64 电脑，重新签名完了之后再在 RT 平板上安装。
- 如果你下载的软件包 Appx 附带了 pfx 或 cer 文件，那就说明它已经重新打包签名过了，可以直接添加信任并安装。
- 如果是依赖包（VCLib，WinJS，UI.Xaml）这种的，那么一般不需要重新打包签名，是微软签名，能直接安装就直接安装没事；如果不信任就添加信任再安装。

{% endfolding %}

## 传统桌面组件被砍

### 限制描述

   在 Windows RT 系列当中，一些桌面组件被移除。一些组件甚至早期版本还有，到了正式版就没有了，如`mshta`。
   > 微软官方说法，Windows Media Player 不适用于 Windows RT 8.1。

### 绕过的方法

   根据圈内其他大佬的研究成果，我们可以从微软的服务器上“扒”下来 Repair Content，Repair Content 是系统的零散组件，理论上可以用来“搓”出来一个能启动的系统。
   1. 我们先去[9826技术栖息地的灵感簿](http://9826hzg.ysepan.com)，翻到“其他零散资源（系统）→WindowsRT”，然后下载这里的`rt9600wmp.zip`。Repair Content 的版本应该是要和系统的版本完全对上的，目前只有 Windows RT 8.1 （内部版本 9600）可以用这里下载的文件。
   2. 然后我们需要解压缩到一个 U 盘或者存储卡里，里面一共有三个文件夹，我们首先用到的就是里面的 rt9600wmp 文件夹。然后我们在 Windows RT 设备上要进入另外一个系统，可以是 WinRE 恢复环境。在那个系统里，我们用 dism /add-package 用法来添加功能，一共需要添加三个文件，但是压缩包里的文件不能缺，也就是指令有三行，但是文件还有一些别的，要放在一起。
   三行指令分别是：
   `dism /image:C: /add-package /PackagePath:microsoft-windows-mediaplayback-oc-package~31bf3856ad364e35~arm~~6.3.9600.16384.mum`
   `dism /image:C: /add-package /PackagePath:microsoft-windows-mediaplayer-package~31bf3856ad364e35~arm~~6.3.9600.16384.mum`
   `dism /image:C: /add-package /PackagePath:microsoft-windows-mediaplayer-package-ua~31bf3856ad364e35~arm~~6.3.9600.16384.mum`
   （其中`/image:`和`/PackagePath`参数仅作示例，具体换成你实际的路径，你可以提前用`cd /d`指令来事先切换到 .mum 文件所在的文件夹）
   3. 如果添加的时候发现添加失败，比如“找不到引用的汇编”，那么我们需要将 rt9600wmp_add 文件夹里的东西和 rt9600wmp 的合并，可以用指令直接复制：
   `xcopy /e D:\rt9600wmp_add\* D:\rt9600wmp\`
   （命令中 D:\ 作为示例，具体替换为你自己的路径）
   然后还需要再敲一遍三条指令。
   4. 最后，我们回到 Windows RT 8.1 系统当中，将“RT8.1 WMP组件进一步修复”的文件放进 C: 盘的对应文件夹当中。
   5. 完事。
   目前只有 Windows Media Player 值得修复且修复方式相对成熟，对于便笺（StickyNotes）的“补回”仍在研究当中，目前会导致“便笺只能创建，无法删除，别的调颜色之类的都干不了”。

## 传统桌面功能被限制

### 限制描述

   你自定义的开机自启都没用，注册表或者 `shell:startup` 都没用，只能注册成服务或者放进任务计划 `taskschd.msc`。
   从 Build 8288 后的某个版本开始，无法远程桌面被连接，怎么都无法，只能连接别人。据说源代码级别就被 `ifdef` 地阻止了。

   无法解决。~~除非买下微软~~