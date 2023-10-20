---
layout: post
title: "Centos7 编译安装 Nginx"
categories: ["技术"]
tags: ["Nginx"]
---

* 目录
{:toc .toc}

## 说明

仅凭记忆记录安装步骤。

以后可能会补充详细过程，更大可能不会。

## 过程

安装过程参考：[源码安装 Nginx](https://www.tecmint.com/install-nginx-from-source/)

执行配置的时候，结合了：[Centos 7.9 tar包编译安装 nginx1.24.0](https://blog.51cto.com/kele/7230473)

最终形成的执行配置如下：

```bash
./configure --user=nginx --group=nginx --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --with-http_ssl_module --with-stream --with-http_stub_status_module --with-http_gzip_static_module --with-mail=dynamic --with-pcre --with-zlib=/root/nginx/zlib-1.3
```

执行配置主要是对模块的选择，模块的具体作用可参考官方手册。[^1]

[^1]: [安装 Nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/)