---
layout: post
title: "JavaEE 中 web.xml 文件的作用"
categories: 技术
tags: ["web.xml"]
---

* 目录
{:toc .toc}

每个 JavaEE 工程中都有 web.xml 文件，那么它的作用是什么呢? 它是每个 JavaEE 工程都必须的吗？

一个 web 工程中可以没有 web.xml 文件，也就是说 web.xml 文件并不是 web 工程必须的。

web.xml 文件是用来初始化配置信息：比如 Welcome 页面 servlet、servlet-mapping、filter、listener 启动加载级别等。

当你的 web 工程没用到这些时，你可以不用 web.xml 文件来配置你的 Application。

每个 xml 文件都有定义它书写规则的 Schema 文件。也就是说 JavaEE 定义的 web.xml 所对应的 Xml Schema 文件中定义了多少种标签元素，web.xml 中就可以出现多少种它所定义的标签元素，也就具备哪些特定的功能。

web.xml 的模式文件是由 Sun 公司定义的，每个 web.xml 文件的根元素为 \<web-app\> 中，必须标明这个 web.xml 使用的是哪个模式文件。如：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5"
xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
</web-app>
```

web.xml 的模式文件中定义的标签并不是定死的，模式文件也是可以改变的。一般来说随着 web.mxl 模式文件的版本升级，里面定义的功能会越来越复杂，标签元素的种类肯定也会越来越多，但有些不是很常用的，我们只需记住一些常用的并知道怎么配置就可以了。


下面列出 web.xml 我们常用的一些标签元素及其功能：

## 指定欢迎页面

```xml
<welcome-file-list>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>index2.jsp</welcome-file>
</welcome-file-list>
```

> 指定了2个欢迎页面。显示时按顺序从第一个找起，如果第一个存在就显示第一个，后面的不起作用。如果第一个不存在就找第二个，以此类推。

关于欢迎页面：

访问一个网站时默认看到的第一个页面就叫欢迎页，一般情况下是由首页来充当欢迎页的。

一般情况下，我们会在 web.xml 中指定欢迎页。

但 web.xml 并不是一个 Web 的必要文件，没有 web.xml 网站仍然是可以正常工作的。

只不过网站的功能复杂起来后，web.xml 的确有非常大用处，所以默认创建的动态 web 工程在 WEB-INF 文件夹下面都有一个 web.xml 文件。

## 命名与定制 URL

我们可以为 Servlet 和 JSP 文件命名并定制 URL，其中定制 URL 是依赖命名的，命名必须在定制 URL 前。

下面拿 Servlet 来举例：

### 为 Servlet 命名

```xml
<servlet>
    <servlet-name>first_servlet</servlet-name>
    <servlet-class>org.java.FirstServlet</servlet-class>
</servlet>
```

### 为 Servlet 定制 URL

```xml
<servlet-mapping>
    <servlet-name>first_servlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

## 定制初始化参数

可以定制 Servlet、JSP、Context 的初始化参数，然后可以在 Servlet、JSP、Context 中获取这些参数值。

下面用 Servlet 来举例：

```xml
<servlet>
    <servlet-name>first_servlet</servlet-name>
    <servlet-class>org.java.FirstServlet</servlet-class>
    <init-param>
          <param-name>userName</param-name>
          <param-value>Adam</param-value>
    </init-param>
    <init-param>
          <param-name>e-mail</param-name>
          <param-value>1@2.com</param-value>
    </init-param>
</servlet>
```

经过上面的配置，在 Servlet 中能够调用 `getServletConfig().getInitParameter("param_name") ` 获得参数名对应的值。

## 指定错误处理页面

可以通过 异常类型或错误码来指定错误处理页面，例如：

```xml
<error-page>
    <error-code>404</error-code>
    <location>/404.jsp</location>
</error-page>

<error-page>
    <exception-type>java.lang.Exception<exception-type>
    <location>/exception.jsp<location>
</error-page>
```

## 设置过滤器

比如设置一个编码过滤器，过滤所有资源。

```xml
<filter>
    <filter-name>UTF8CharSetFilter</filter-name>
    <filter-class>org.java.UTF8CharSetFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>UTF8CharSetFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```


## 设置监听器
```xml
<listener>
    <listener-class>org.java.InitListener</listener-class>
</listener>
```

## 设置会话 Session 过期时间
其中时间以分钟为单位，假如设置 60 分钟超时。

```xml
<session-config>
    <session-timeout>60</session-timeout>
</session-config>
```

除了这些标签元素之外，还可以往 web.xml 中添加很多标签元素，由于不常用省略。

---

原文：
[web.xml 文件的作用](http://www.cnblogs.com/yqskj/articles/2233061.html)
