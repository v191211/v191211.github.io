---
layout: post
title: "SSH 登录后进入指定目录"
categories: ["技术"]
tags: ["Linux"]
---

* 目录
{:toc .toc}

## 需求

所有用户通过 SSH 登录到服务器时，默认进入到 /data 目录下。

## 解决

在 /etc/profile.d 目录下创建 cd_data.sh 文件，内容：

```bash
# cd /data when user login
cd /data
```

## 原理

/etc/profile.d 目录下的所有脚本，会在用户登录初始化环境时执行。[^1] [^2] [^3] [^4]

## 扩展

* /etc/profile: 系统初始化文件，登录 shell 时执行
* ~/.bash_profile: 个人初始化文件，登录 shell 时执行
* ~/.bashrc: 每次启动一个交互 shell 时都会执行

所以，全局的配置文件可以放到 /etc/profile 文件中。

[^1]: [如何在 CentOS7 系统中为所有用户设置路径？](https://unix.stackexchange.com/questions/409126/how-to-set-path-for-all-users-on-centos-7){:target="_blank"}

[^2]: [.bashrc, .bash-profile, .profile 之间的区别](https://www.baeldung.com/linux/bashrc-vs-bash-profile-vs-profile){:target="_blank"}

[^3]: [.bashrc, .bash_profile, .environment 之间有什么区别？](https://stackoverflow.com/questions/415403/whats-the-difference-between-bashrc-bash-profile-and-environment){:target="_blank"}

[^4]: [对比在 /etc/environment 与 .profile 中设置 PATH 变量](https://askubuntu.com/questions/866161/setting-path-variable-in-etc-environment-vs-profile){:target="_blank"}