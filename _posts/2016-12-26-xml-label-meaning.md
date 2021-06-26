---
layout: post
title: XML 文件中标签的含义
categories: 技术
tags: XML
---

* 目录
{:toc .toc}

例，如下 xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                          http://www.springframework.org/schema/beans/spring-beans-2.0.xsd">
       ... ...
</beans>
```

beans：XML 的根节点。

xmlns：XML name space（XML名称空间），XML 文件的标签名称都是自定义的，自己写的和其他人定义的标签很有可能会重复命名，而功能却不一样，所以需要加上一个名称空间来区分这个 xml 文件和其他的 xml 文件，类似于 java 中的 package。

xmlns:xsi：xml schema instance 是指 xml 文件遵守的 xml 规范。

xsi:schemaLocation：指具体用到的 schema 资源，schema 和 dtd 一样是约束 XML 文件的。

---

原文：[xml文件中xsi等等都是什么意思？](http://blog.sina.com.cn/s/blog_4b6f8d150100nx3e.html)