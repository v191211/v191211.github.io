---
layout: post
title: "Centos7 安装 Percona server for MySQL"
categories: ["技术"]
tags: ["Mysql", "Percona"]
---

* 目录
{:toc .toc}
## 过程

使用的是 RPM 包安装方式，参考官方文档即可。

唯一需要特别注意的是，官方文档上面给出的下载地址是 RHEL 8 的地址，不适用于 Centos7，需要自己到下载页面选择 RHEL 7版本然后按照指令进行安装。

安装时注意顺序（不注意也可，缺少某个 rpm 时会有提示），印象中顺序是：

```bash
sudo rpm -ivh percona-server-shared-compat percona-server-shared percona-server-devel percona-server-client percona-server-server
```

安装完后，需要更新 root 密码 [^2]

## RPM 包文件 [^1]

| 包                           | 内容                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| percona-server-server        | 服务                                                         |
| percona-server-debuginfo     | debug 标志                                                   |
| percona-server-client        | 命令行客户端                                                 |
| percona-server-devel         | 头文件，使用 client 库编译文件时需要                         |
| percona-server-shared        | client 共享库                                                |
| percona-server-shared-compat | 编译旧版本 client 库用到的共享库，包含以下包： libmysqlclient.so.12, libmysqlclient.so.14, libmysqlclient.so.15, libmysqlclient.so.16, and libmysqlclient.so.18.<br />这些软件包不包含在 Red Hat Enterprise Linux 9 及其衍生版本的下载中。 |
| percona-server-test          | 一组测试 Percona Server for MySQL 的工具                     |

## 参考

[下载 RPM 包安装 Percona server for MySQL](https://docs.percona.com/percona-server/8.0/yum-download-rpm.html)



[^1]: [RPM 包文件](https://docs.percona.com/percona-server/8.0/yum-files.html)
[^2]: [安装 Percona server for MySQL 后设置](https://docs.percona.com/percona-server/8.0/post-installation.html)