---
layout: post
title: "Centos7 安装 LEMP"
categories: ["技术"]
tags: ["LEMP"]
---

* 目录
{:toc .toc}

## 说明

仅凭记忆记录安装步骤。

以后可能会补充详细过程，更大可能不会。

## LEMP 释义

L: Linux 操作系统

E: Nginx (引擎)，HTTP 和反向代理服务器

M: MySQL 或 MariaDB 数据库

P: PHP 程序语言

## 过程 [^1] [^2] [^3]

### 安装 Nginx

参考文章中使用的是 yum 包安装方式，也可以使用手动安装方式，参考 [Centos7 编译安装 Nginx]({{ site }}/2023/10/19/compile-install-nginx-on-centos7)

### 安装 Mysql

参考文章中使用的是 MariaDB，实际安装时使用的是 Percona Server for MySQL，参考 [Centos7 安装 Percona Server for MySQL]({{ site }}/2023/10/19/percona-server-for-mysql-on-centos7)

### 安装 PHP [^4]

Centos7 中自带的 PHP 版本是 5.4，已经停止支持了。

使用的是 Remi 仓库。

### 配置 Nginx 支持 PHP 程序

参考文章中配置的所有 .php 结尾的程序都使用 PHP 处理



[^1]: [Centos7 安装 LEMP](https://www.hostinger.com/tutorials/how-to-install-lemp-centos7)
[^2]: [Centos7 安装 LEMP](https://linuxize.com/series/install-lemp-stack-on-centos-7/)
[^3]: [Centos7 安装 LEMP](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7#step-4-configuring-nginx-to-process-php-pages)

[^4]: [Remi RPM 仓库配置](https://rpms.remirepo.net/wizard/)