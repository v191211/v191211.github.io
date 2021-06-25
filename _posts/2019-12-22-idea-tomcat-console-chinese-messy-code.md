---
layout: post
title: "IDEA TOMCAT 控制台中文乱码"
categories: 技术
tags: ["IDEA 乱码"]
---

* 目录
{:toc .toc}

# 环境

Windows10 家庭中文版

Intellij IDEA 2019.2.4(Ultimate Edition)

Tomcat-7.0.99

# 问题

IDEA，配置 TOMCAT 并启动，控制台打印中文乱码，如：淇℃伅

# 解决

修改 TOMCAT 下 conf 目录中的 logging.properties 文件

```properties
#java.util.logging.ConsoleHandler.encoding = UTF-8
java.util.logging.ConsoleHandler.encoding = GBK
```

































