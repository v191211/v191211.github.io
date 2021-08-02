---
layout: post
title: "配置安装在 Virtual box 中的 CentOS"
categories: ["技术"]
tags: ["Linux", "CentOS", "Virtual box"]
---

* 目录
{:toc .toc}

## Virtual box 上安装 CentOS 7.x

参考：[图示CentOS 7.0 安装](https://www.tecmint.com/centos-7-installation/)



## 更换 yum 源为清华 yum 源

参考：[CentOS 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/centos/)



## 安装 Virtual box 扩展插件

```bash
# 以 root 用户执行下列命令：
$ yum install update
$ yum install kernel-devel kernel-headers
$ yum install gcc
```

然后选择 VirualBox 菜单中的 Devices > Insert Guest Additions CD image...

注意：

如果出现错误 'Failed to set up service vboxadd, please check the log file /var/log/VBoxGuestAdditions.log for details.'，参考以下解决方案进行解决：

* [VirtualBox虚拟机CentOS安装增强功能Guest Additions](https://www.jianshu.com/p/7c556c783bb2)
* [VirtualBox虚拟机CentOS安装增强功能Guest Additions](https://www.centos.bz/2017/07/virtualbox-vhost-centos-install-guest-additions/)

参考：

* [VirtualBox中CentOS 屏幕分辨率进行修改](http://blog.csdn.net/lhd435940424/article/details/38333781)
* [VirtualBox安装CentOS7后安装VirtualBox增强工具](https://www.sunyann.com/centos/)



## 主机连接 Virtual box 虚拟机
参考：[virtualbox桥接网络配置--CentOS](http://www.cnblogs.com/sgamerv/p/centos_virtualbox_bridge_config.html)



## 其它配置

**改变默认的 sudo 密码超时时间**

```bash
$ sudo visudo
# 添加下列行：
Defaults timestamp_timeout=30 # 30 是新的超时时间，以分钟为单位。
# 完全禁用输入 sudo 密码：
$ Defaults:<user_name> !authenticate
```

参考：[更改默认 sudo 密码超时时间](https://unix.stackexchange.com/a/382061)



**创建用户**

```bash
$ adduser <username>
```

**更新（设置）用户密码**

```bash
$ passwd <username>
```

**给用户添加 sudo 权限**

方式一：

```bash
$ usermod -aG wheel <username>
```

CentOS 系统默认 wheel 组中的成员具有 sudo 权限。



方式二：

```bash
$ visudo
# 添加下列代码：
$ <username> ALL=(ALL) ALL
```



测试 sudo 权限：

```bash
$ sudo ls -la /root
```

参考：

* [Linux 中创建 sudo 用户](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-centos-quickstart)
* [CentOS7 中添加用户并授予 root 权限](https://www.liquidweb.com/kb/how-to-add-a-user-and-grant-root-privileges-on-centos-7/)



**添加用户到多个组**

```
$ usermod -aG group1,group2 username
```

参考：[Linux 中添加用户到多个组](https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)



**查看所有用户**

```bash
$ getent passwd
```



**查看所有组**

```bash
$ getent group
```



**查看指定用户所有组**

```bash
$ getent group | grep <username>
```

参考：[查看所有用户和他们的组](https://serverfault.com/a/355294)



**查看用户所属组**

```bash
$ groups <username>
```

参考：[查看 Linux 用户所属组](https://www.howtogeek.com/howto/ubuntu/see-which-groups-your-linux-user-belongs-to/)



**查看当前语言**

```bash
$ localectl
```



**查看本地语言列表**

```bash
$ localectl list-locales
```



**更改语言**

```bash
$ sudo localectl set-locale LANG=en_US.UTF-8
```

参考：[设置系统语言](https://www.server-world.info/en/note?os=CentOS_7&p=locale)

