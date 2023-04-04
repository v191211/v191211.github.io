---
layout: post
title: "JavaEE 中 web.xml 配置示例"
categories: ["技术"]
tags: ["Servlet"]
---

* 目录
{:toc .toc}

![]({{ site.url }}/assets/images/posts/web.xml_.png)

`web.xml` 配置文件是用来描述 web 应用如何部署的。以下5个 `web.xml` 配置文件的例子，仅供自己参考。

## 1. Servlet 3.1

Java EE 7 XML schema，名称空间是 http://xmlns.jcp.org/xml/ns/javaee/

```
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
</web-app>
```

## 2. Servlet 3.0

Java EE 6 XML schema，名称空间是 http://java.sun.com/xml/ns/javaee

```
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
          version="3.0">
</web-app>
```

## 3. Servlet 2.5

Java EE 5 XML schema，名称空间是 http://java.sun.com/xml/ns/javaee

```
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
          http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
          version="2.5">
</web-app>
```

## 4. Servlet 2.4

J2EE 1.4 XML schema，名称空间是 http://java.sun.com/xml/ns/j2ee

```
<web-app xmlns="http://java.sun.com/xml/ns/j2ee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
          http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
          version="2.4">

  <display-name>Servlet 2.4 Web Application</display-name>
</web-app>
```

## 5. Servlet 2.3

J2EE 1.3 DTDs schema。这个版本的 `web.xml` 文件太古老了，建议升级为更高版本。

```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Servlet 2.3 Web Application</display-name>
</web-app>
```

>  **Java Servlet 重大改变**
>
> 访问 [Servlet wiki](http://en.wikipedia.org/wiki/Java_Servlet) 页面查看 Servlet2.3 到 3.0 重大改变摘要。

---

原文：
[web.xml 部署描述符示例](http://www.mkyong.com/web-development/the-web-xml-deployment-descriptor-examples/)
