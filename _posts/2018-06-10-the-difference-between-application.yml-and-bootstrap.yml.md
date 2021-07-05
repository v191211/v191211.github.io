---
layout: post
title: "SpringBoot 中 application.yml 和 bootstrap.yml 文件的区别"
categories: ["技术"]
tags: ["SpringBoot"]
---

* 目录
{:toc .toc}

## 区别1

**bootstrap.yml** 或 **bootstrap.properties** 仅用于或需要用于，如果你使用 **Spring Cloud** 并且配置文件存储在远程配置服务器（例如：Spring Cloud 服务）

官方文档：

> Spring Cloud 应用由 bootstrap 上下文创建，该上下文是这个应用的父级上下文。
>
> 它是开箱即用的，负责从外部资源中加载配置信息和解密本地外部配置文件的配置信息。

注意：**bootstrap.yml** 或 **bootstrap.properties** 可以包含额外的配置（例如：默认的配置），但通常只需要写你自己需要的配置项。

它通常都会包含这两个属性：

* 配置服务的位置（**spring.cloud.config.uri**）。
* 应用名称（**spring.application.name**）。

在启动时，Spring Cloud 发送一个带有应用名称的 HTTP 调用到配置服务，然后接收返回回来的该应用的配置信息。

**application.yml** 或 **application.properties** 包含标准的应用配置，通常是默认配置；在项目启动期间收到的任何配置都会覆盖掉默认配置。

## 区别2
**bootstrap.yml 是在 application.yml 之前加载。**

通常做如下使用：

* 当使用 Spring Cloud 配置服务时，你应该在 bootstrap.yml 中指定 **spring.application.name** 和 **spring.cloud.config.uri**。
* 一些加密/解密信息。

严格意义上讲 **bootstrap.yml** 是被父级 **Spring ApplicationContext** 加载。而该父级 **Spring ApplicationContext** 在使用 **application.yml** 之前就被加载了。

---

原文：[SpringBoot 中 application.yml 和 bootstrap.yml 配置文件的区别](https://stackoverflow.com/a/43269161/4612522)