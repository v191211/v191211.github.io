---
layout: post
title: "深入理解设计模式"
categories: ["技术"]
tags: ["设计模式"]
---

* 目录
{:toc .toc}
昨天读完了《深入理解设计模式》（《Dive Into Design Patterns》）这本书，还是有颇多感触的，这本书是由 Alexander Shvets 编写，主要对 22 种设计模式做了一个直观、形象并且足够简单的介绍。这本书的结构非常清晰，主要分为前置部分（介绍设计模式的出现的缘由以及其设计所应当遵循的原则）与正文（将 22 种设计模式分为了三类，并对每一个设计模式做了充分的介绍）两部分，其中正文部分是本书的重点，下面对这本书做一个必要的总结，以期在以后查阅该书时能够快速索引。[^1] [^2]

## 关于本书

起初这本书是在浏览这个网站 [Source Making](https://sourcemaking.com/) 所注意到的，这个网站是代码洁癖的福音，因为里面这个网站对设计模式、反模式、重构以及 UML 做了一个全面的介绍，致力于让开发者能够写出更加漂亮的代码。因为最近打算深入了解下 TCP 在 Linux 操作系统的实现，但是《TCP Socket In C》的作者建议在读该书前要对设计模式有所了解，虽然本科期间开设了关于设计模式的课程，但无奈当时我太划水，没怎么听，而且正好最近对设计模式也很感兴趣，所以那就一石三鸟，通过读这本书弥补一下本科的遗憾，满足一下我的兴趣、以及了解一下《TCP Socket In C》这本书的前置知识吧！

## 关于作者

Alexander Shvets，俄国人，在领英上的信息显示其硕士毕业于莫斯科国立鲍曼技术大学（Bauman Moscow State Technical University），在很多个公司就职过，现在是 ScanToBuy 公司的CTO。

## 内容结构

### 前置部分

前置部分包含三章，分别是：

- 面向对象介绍，其中分为：

  - 面向对象基础：一些诸如类、对象、继承等 OOP 基础知识，乏善可陈；
  - 面向对象核心思想：抽象、多态、封装、继承。本科知识、乏善可陈。需要注意实现与继承的连线画法上的区别，因为正文部分会有大量的箭头，现在不清楚后面读起来也容易搞混，耽误不必要的时间；
  - 对象之间的关系：对象之间的关系，即关联、依赖、组合、聚合。需要注意它们连线画法上的区别。

- 设计模式介绍，其中分为：

  什么是设计模式：介绍了设计模式的结构、分类以及发明者（GoF）;

  为什么要学习设计模式；

- **软件设计原则**，这部分比较重要，也是各个设计模式出现的初衷：**当前软件设计面临什么问题、我们应当如何解决这些问题、以及解决这些问题应当遵循哪些原则**。其中分为：

  - 设计原则：什么才是好的设计？作者认为应当是：

    封装变化：主要的原则是最小化变化所带来的影响，有方法层面的封装以及类层面的封装；

    面向接口编程、而非面向实现编程：接口可以理解为没有成员变量的抽象类，面向接口编程的抽象度会更高；

    使用组合来代替继承所带来的问题：继承可能带来维护灾难，使用组合关系能够更好地解决这个问题，举个例子，TCP 里的 Socket Base 类有一个域用来存放拥塞控制算法，即我把 Vegas 作为我的拥塞控制算法（has）；而之前则是直接基于基类往下继承，即我是 Vegas 拥塞控制算法（is）。在 NS3 的技术文档中也描述了这种实现方式：

    > Before, a congestion control was considered in the code as a stand-alone TCP. In software-engineering terms, the inheritance relation between a TCP congestion algorithm, for example TcpNewReno , and the main TCP class TcpSocketBase , logically implied that “TcpNewReno was-a TcpSocketBase ”. The change consisted in reverting this paradigm in a more sound statement: “TcpSocketBase has TcpNewReno as congestion control algorithm”, which basically translates in avoiding the inheritance relation between these classes, and writing an interface to exchange data between sockets and congestion control modules. Such modularity is already employed in real-world stacks (a famous example is the modularization of congestion control algorithms in Linux).

    这种设计有很多缺点，此不赘述。

  - SOLID 原则：SOLID 原则是五个不同原则的简称，它们分别是：单一责任原则（Single Responsibility Principle）、开闭原则（Open/Closed Principle）、里氏代换原则（Liskov Substitution Principle）、接口分离原则（Interface Segregation Principle）以及依赖倒置原则（Dependency Inversion Principle）。下面对这五个原则进行逐一的介绍：

    **S**ingle Responsibility Principle：每一个类应当只负责其应当负责的部分，当软件变得逐渐庞大、责任越来越多时，就应当将责任分离以防止原类变得过于庞大；

    **O**pen/Closed Principle：类应当对于扩展开放、对于更改封闭（高内聚、低耦合）；

    **L**iskov Substitution Principle：这个原则比较有意思，它的出现是基于多态的考虑（当扩展一个类时，应当记住我们在任何时刻都可以传递一个子类的对象在基类的位置，所以我们应当时刻考虑我们的设计基于此所带来的影响）。这个原则也可以细分为几个子原则：相对于父类来说，子类的函数参数应当更加抽象，不能加强前置。子类的返回值、报出异常应当更加具象，不能削弱后置。子类不应该加强前置条件，尽量添加新函数，而不是重写原有函数；

    **I**nterface Segregation Principle：把不相关的函数分离成多个接口，而不是全部放在一个接口里，这会导致实现该接口的类也要实现那些它们可能用不到的接口；

    **D**ependency Inversion Principle：高级类（业务类）应当依赖于低级类（轮子类）的抽象，而不是直接依赖，这会导致耦合关系。

### 正文部分

正文部分分为三部分，分别是：

- 创建型设计模式：包含工厂模式、抽象工厂模式、建造者模式、原型模式、单例模式；
- 结构型设计模式：包含适配器模式、桥接模式、复合模式、装饰器模式、外观模式、享元模式、代理模式；
- 行为型设计模式：包含责任链模式、命令模式、迭代器模式、中介者模式、备忘录模式、观察者模式、状态模式、策略模式、模板方法模式、访问者模式。

在介绍每一个单一的设计模式时，也分为一下几部分：

- 问题
- 解决方案
- UML 结构
- 伪代码
- 应用场景
- 如何实现
- 优缺点
- 和其他设计模式的关系

总的来说，从 UML 的角度，所有的设计模式可以概括为：

[设计模式速查手册](http://www.lug.or.kr/files/cheat_sheet/design_pattern_cheatsheet_v1.pdf)

## 附录

**书中的箭头**

本书中的继承、实现、关联、依赖、组合、聚合分别使用六种箭头表示：

- 继承：三角实线箭头
- 实现：三角虚线箭头
- 关联：普通实线箭头
- 依赖：普通虚线箭头
- 组合：实心菱形箭头
- 聚合：虚心菱形箭头



[^1]: [深入理解设计模式](https://tristone13th.github.io/archivers/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F){:target="_blank"}

[^2]: 对原文章内容有所修改。