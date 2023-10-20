---
layout: post
title: "Centos7 安装 PhpMyAdmin"
categories: ["技术"]
tags: ["PhpMyAdmin"]
---

* 目录
{:toc .toc}

## 说明
仅凭记忆记录安装步骤。

以后可能会补充详细过程，更大可能不会。

## 过程

Centos7 仓库只能安装较低版本的 PhpMyAdmin，Remi 仓库提供了更高版本。

首先安装 LEMP 环境，安装过程参考：[Centos7 安装 LEMP]({{ site.url }}/2023/10/19/install-lemp-stack-on)

安装 PHP 使用的是 Remi 仓库。[^1]

同时安装了 php-mbstring 和 php-gettext 扩展，PhpMyAdmin 需要使用 php-mbstring 扩展命令：[^2]

```bash
yum install php-fpm php-opcache php-cli php-gd php-curl php-mysql php-mbstring php-gettext
```

安装 PhpMyadmin 同样使用的 Remi 仓库。[^3]

安装好后根据 [Centos7 安装 PhpMyAdmin 集成 Nginx](https://linuxize.com/post/how-to-install-phpmyadmin-with-nginx-on-centos-7/) 做的 nginx 反向代理配置。

同时，设置了 Authentication Gate。[^4] [^5]

[^1]: [Remi RPM 仓库配置](https://rpms.remirepo.net/wizard/)
[^2]: [phpMyAdmin Error: The mbstring extension is missing](https://stackoverflow.com/questions/30047169/phpmyadmin-error-the-mbstring-extension-is-missing-please-check-your-php-confi)
[^3]: [phpMyAdmin version 5](https://blog.remirepo.net/post/2020/01/22/phpMyAdmin-version-5-en)
[^4]: [安装并增强 PhpMyAdmin 安全](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-with-nginx-on-a-centos-7-server)
[^5]: [安装并增强 PhpMyAdmin 安全](https://community.hetzner.com/tutorials/install-and-secure-phpmyadmin-with-nginx-on-centos-7)
