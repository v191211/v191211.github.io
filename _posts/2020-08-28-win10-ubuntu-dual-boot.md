---
layout: post
title: "Dell Inspiron 5488 双硬盘安装 Win10 Ubuntu 双系统"
categories: 技术
tags: Linux
---

* 目录
{:toc .toc}
## 阅读说明

记录**正确的安装流程**和**需要注意的事项**，并不提供具体的细节（可自行Google）。

因此并非是一篇可以拿来即用的文章，但根据这篇文章的安装顺序和注意事项进行双系统的安装可以节省你大量宝贵的时间。

我大概用了12h左右的时间完成的安装，时间主要花费在有很多需要注意的地方在大多数教程中并未提起，遇到一个解决一个。

Windows系统和Ubuntu系统全是英文版本，因此过程中出现的错误提示信息我会翻译成中文，和中文版安装时的错误提示信息并不能完全一样，对照着看意思一样应该就属同一个错误。



## 硬件环境

| 品牌 | Dell                                                       |
| ---- | ---------------------------------------------------------- |
| 型号 | Inspiron 5488                                              |
| CPU  | i7-8565U                                                   |
| 内存 | 32GB。`PS：官方原版为8GB，最高支持升级至32GB`              |
| 硬盘 | 256GB固态硬盘+1TB机械硬盘。`PS：官方原版为256GB固态硬盘。` |
| 显卡 | NVIDIA GeForce MX250                                       |



## 软件环境

| Windows系统       | Windows10 Pro N 1909 |
| ----------------- | -------------------- |
| Linux系统         | Ubuntu 20.04.01      |
| USB系统盘制作工具 | rufus-3.11           |
| 引导模式          | UEFI                 |



## 作业记录

### 一：关闭磁盘加密

打开 Control Panel （控制面板） > System and Security （系统和安全） > BitLocker Drive Encryption（磁盘加密），点击 Turn off BitLocker（关闭磁盘加密）。



注：加密的磁盘才会有关闭选项，如果找不到关闭选项，可能是磁盘未进行加密。

关闭磁盘加密非常耗时，系统右下角有可以查看进度的图标，如果没有可以通过命令查看完成百分比，例如查看 D 盘关闭磁盘加密进度：

打开 powershell 或 cmd 命令，运行 `manage-bde -status d:`



>  **注意：**如果存在加密的磁盘，安装 Ubuntu 时会提示错误：
>
> Turn off BitLocker. This Computer uses Windows BitLocker encryption. You need to turn off BitLocker in Windows before installing Ubuntu. For instructions, open this page on a phone or other device: [help.ubuntu.com/bitlocker](help.ubuntu.com/bitlocker)。
>
> 中文意思为：关闭磁盘加密。电脑使用了 Windows 的磁盘加密技术，安装 Ubuntu 系统前需要先进行关闭。操作方法可以查看 [help.ubuntu.com/bitlocker](help.ubuntu.com/bitlocker)页面。



### 二：设置BIOS

开机按F2键，进入BIOS界面。

1. 选择 Secure Boot > Secure Boot Enable，将 Enabled 改为 Disabled。
2. 选择 Security > PPT security，取消勾选 PPT On。
3. 选择 System Configuration > SATA Operation，选择 AHCI。
4. 选择 General > Boot Sequence > Boot List Option，选择UEFI。



>  **注意：**如果不设置 SATA Operation 为 AHCI 模式，而先安装了 Windows 系统，这时 Windows 系统使用的是 RST ( Intel Rapid Storage，因特尔磁盘快速存储技术)，在安装 Ubuntu 时可能会出现错误提示：
>
> Turn off RST. This computer uses Intel RST(Rapid Storage Technology). You need to turn off RST before installing Ubuntu. For instructions, open this page on a phone or other device: 
>
> 中文意思为：关闭 RST，当前电脑使用了 RST 技术，安装 Ubuntu 前需要进行关闭，操作方法可查看 [help.ubuntu.com/rst](help.ubuntu.com/rst)



### 三：安装 Windows 系统

安装 Windows 系统到 256GB 硬盘上面。



### 四：关闭 Windows 快速启动

Control Panel（控制面板） > Hardware and Sound（硬件和声音） > Power Options（电源选项） > Choose what the power buttons do（电源按钮）> Change settings that are currently unavailable （更改当前不可用的选项），取消勾选 Turn on fast startup(recommended) （开启快速启动（推荐））按钮。



### 五：划分 Ubuntu 系统空间

进入 Windows 系统，我的电脑（右键） > 管理 > 磁盘管理 > 需要进行压缩的磁盘（右键） > 压缩磁盘，选择合适的大小。



>  **注意：** Ubuntu 的 /boot 分区，必须和 Windows 系统在同一物理磁盘上面。其它分区无要求，划分到任意磁盘都可以。
>
> 我的分区方案是：/boot（划分10GB空间） 和 Windows 放在 256GB 固态硬盘，swap（划分16GB空间） 放在 256GB 固态硬盘，/home（划分74GB）放在256固态硬盘。/（划分100G）放在 1TB 机械硬盘。



### 六：安装 Ubuntu 系统

安装 Ubuntu 系统。



>  **注意：**
>
> 1. 再次强调，Ubuntu 的 /boot 分区，必须和 Windows 系统在同一物理磁盘上面。其它分区无要求，划分到任意磁盘都可以。
> 2. Ubuntu 的安装过程中要联网并勾选安装第三方软件、Graphcis 和 Wifi 等功能，否则启动时可能一直停留在 Logo 画面。



至此，安装 Windows 和 Ubuntu 双系统到 Dell Inspiron 5488 双硬盘完成。

---

参考：

[如何安装 Ubuntu 20.04 和 Windows 10 双系统 UEFI - GTP 模式](https://www.youtube.com/watch?v=aKKdiqVHNqw&list=PLME0oHvpJ2pWFzvUhpK1nTJ_dn1W3ktpp&index=2&t=251s)

[在启用 RST 的电脑上安装 Ubuntu](https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-with-intel-r-rst-enabled/15347)

[在开启磁盘加密的 Windows 系统上安装 Ubuntu](https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338)