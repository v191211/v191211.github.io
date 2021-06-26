---
layout: post
title: "解决 Maven 仓库镜像被锁定"
categories: ["技术"]
tags: ["Maven"]
---

* 目录
{:toc .toc}
## 问题

执行 mvn 命令时错误，提示：

**Could not transfer artifact x from/to maven-default-http-blocker (http://0.0.0.0/): Blocked mirror for repositories: [x, default, releases+snapshots), releases (x, default, releases+snapshots)]**



## 问题原因

从 3.8.1 版本，Maven 引入了一条锁定所有使用 HTTP 协议（不安全连接）仓库的默认配置。



## 解决方法

### 方法一

降级 Maven 版本为 [**3.6.3**](https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/)



### 方法二

在 ~/.m2/settings.xml 配置文件 \<mirror\> 标签内添加 \<blocked\> 标签，值为 false，例如：

```xml
<mirror>
  <id>insecure-repo</id>
  <mirrorOf>external:http:*</mirrorOf>
  <url>http://www.ebi.ac.uk/intact/maven/nexus/content/repositories/ebi-repo/</url>
  <blocked>false</blocked>
</mirror>
```



### 方法三

使仓库支持 HTTPS 协议，修改 ~/.m2/settings.xml 配置文件中的镜像 URL 为 HTTPS 方式的连接。




---

参考：

[Maven构造失败 - 依赖异常](https://stackoverflow.com/a/67121849/4612522)

