---
layout: post
title: "配置安装在 Virtual box 中的 Ubuntu"
categories: ["技术"]
tags: ["Linux", "Ubuntu", "Virtual box"]
---

* 目录
{:toc .toc}
## 环境

VirtualBox：Version 5.2.20 r125813 (Qt5.6.2)

Ubuntu：18.04.1-desktop-amd64 (LTS)



## 设置 root 密码

```bash
$ sudo passwd root
```



## 更换 apt-get 源 [^1]

使用[清华大学](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)的源。

apt-get源路径：`/etc/apt/sources.list`

```bash
# 备份源
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 更换源
$ sudo vim /etc/apt/sources.list
# 更新软件包列表
$ sudo apt update
# 更新本机已安装软件包
$ sudo apt upgrade
```



## 安装 Virtual box 扩展功能[^2]

```bash
# 安装 Virtual box 扩展镜像
$ sudo apt-get install virtualbox-guest-additions-iso
# 新建挂载文件夹
$ sudo mkdir /media/vb_guest
# 挂载扩展镜像到文件夹
$ sudo mount -o loop /usr/share/virtualbox/VBoxGuestAdditions.iso /media/vb_guest
# 执行安装程序后，卸载挂载的文件夹
$ sudo umount /media/vb_guest
```



## 查看 Ubuntu 版本

```bash
$ lsb_release -a
```





## 开放 22 端口

```bash
# 更新软件包列表
$ sudo apt update
# 更新本机已安装软件包
$ sudo apt upgrade
# 安装 ssh 服务
$ sudo apt-get install openssh-server
# 防火墙放行 22 端口（也可以直接关闭防火墙）
$ sudo ufw allow 22
```



## 设置鼠标聚焦策略[^3]

```bash
$ gsettings set org.gnome.desktop.wm.preferences focus-mode 'sloppy'
```



窗口聚焦模式表示如何激活窗口，有三种可选的值。

click：表示窗口必须被点击才能被聚焦。

sloppy：表示鼠标进入窗口时聚焦。

mouse：表示鼠标进入窗口时聚焦，鼠标离开窗口时失去焦点。

当鼠标悬停在桌面上，并且没有打开的窗口时，mouse 模式会聚焦到桌面上，而 sloppy 模式不会。

sloppy 模式仅在鼠标悬停在窗口上时才会改变焦点。

不过，在 ubuntu 中无效。



## 更改系统显示语言为中文[^4]

Setting > Region & Language > Formats > United States > Restart



## 禁用自动锁屏

Setting > Privacy > Screen Lock > OFF



## 禁用自动黑屏

Setting > Privacy > Power Saving > Blank screen > Never



## 配置运行 sudo 命令时无需输入密码[^5]

```bash
$ sudo visudo
<user_name> ALL=(ALL) NOPASSWD:ALL
```



[^1]: [Ubuntu 可更换源列表](http://wiki.ubuntu.org.cn/%E6%A8%A1%E6%9D%BF:18.04source){:target="_blank"}
[^2]: [安装 VirtualBox 扩展到](https://www.tecmint.com/install-virtualbox-guest-additions-in-ubuntu/){:target="_blank"}
[^2]: [如何安装扩展到 VirtualBox 中？](https://askubuntu.com/questions/22743/how-do-i-install-guest-additions-in-a-virtualbox-vm){:target="_blank"}
[^3]: [设置鼠标聚焦策略](https://askubuntu.com/questions/64605/how-do-i-set-focus-follows-mouse){:target="_blank"}
[^4]: [更改 Ubuntu 系统显示语言](https://www.howtogeek.com/howto/17528/change-the-user-interface-language-in-ubuntu/){:target="_blank"}
[^5]: [如何在 Linux 或 Unix 系统中运行 sudo 命令而不需要输入密码](https://www.cyberciti.biz/faq/linux-unix-running-sudo-command-without-a-password/){:target="_blank"}
[^5]: [执行 sudo 命令而无需输入密码](https://askubuntu.com/questions/147241/execute-sudo-without-password){:target="_blank"}
