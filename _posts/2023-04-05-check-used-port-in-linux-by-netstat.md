---
layout: post
title: "使用 netstat 命令查看指定端口占用情况"
categories: ["技术"]
tags: ["Linux 命令", "netstat"]
---

* 目录
{:toc .toc}

```bash
$ netstat -anp | grep :<port_number>
```

> -a: 监听、非监听状态的连接都显示
>
> -n: 显示 ip 地址，而不是域名或用户名
>
> -p: 显示连接所属 PID 和程序名称
