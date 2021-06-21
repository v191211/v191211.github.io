---
layout: post
title: Intellij Idea 常用插件
categories: 技术
tags: ["Intellij Idea", "插件"]
---

* 目录
{:toc .toc}
## Lombok

Java 语言，每次写实体类的时候都需要写一大堆的 setter，getter，如果 bean 中的属性一旦有修改、删除或增加时，需要重新生成或删除 get/set 等方法，给代码维护增加负担，这也是 Java 被诟病的一种原因。

Lombok 则为我们解决了这些问题，使用了 lombok 的注解（@Setter、@Getter、@ToString、@RequiredArgsConstructor、@EqualsAndHashCode、@Data）之后，就不需要编写或生成 get/set 等方法，很大程度上减少了代码量，而且减少了代码维护的负担。

安装完成之后，在应用 Lombok 的时候注意别忘了需要添加依赖。

在 Idea 2020.3. 版本之后，默认集成了 Lombok。



## RestfulToolkit

Spring MVC 网页开发的时候，我们都是通过 requestmapping 的方式来定义页面的 URL 地址的，为了找到这个地址我们一般都是 cmd+shift+F 的方式进行查找。

大家都知道，我们 URL 的命名一个是类 requestmapping + 方法 requestmapping，查找的时候还是有那么一点不方便的，restfultookit 就能很方便的帮忙进行查找。

例如：我要找到 /user/add 对应的controller，那么只要Ctrl+斜杠 ，就能直接定位到我们想要的controller。

这个也是真心方便，当然 restfultookit 还为我们提供的其他的功能。

根据我们的 controller 帮我们生成默认的测试数据，还能直接调用测试，这个可以是解决了我们每次 postman 调试数据时，自己傻傻的组装数据的的操作，这个更加清晰，比在 console 找数据包要方便多了。



## MyBatisX

我们开发中使用 mybatis 时时常需要通过 mapper 接口查找对应的 xml 中的 sql 语句，该插件方便了我们的操作。

安装完成重启IDEA之后，我们会看到code左侧或多出一列小鸟的图标，点击图标我们就可以直接定位到 xml 相应文件的位置。

---

参考：

[IntelliJ IDEA（九） ：酷炫插件系列](https://www.cnblogs.com/jajian/p/8081658.html)

[Intellij Idea 配置 Lombok](https://projectlombok.org/setup/intellij)

