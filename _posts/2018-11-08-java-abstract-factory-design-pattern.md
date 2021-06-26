---
layout: post
title: 抽象工厂模式
categories: 技术
tags: ["设计模式"]
---

* 目录
{:toc .toc}

抽象工厂设计模式属于创建型设计模式，抽象工厂模式同工厂模式非常相像，但它更像是工厂中的工厂。

# 抽象工厂

Java工厂设计模式中有一个单例工厂类，这个类基于提供的输入，使用 `if-else` 或 `switch` 代码块返回不同的子类对象。

在抽象工厂模式中，摆脱了 `if-else` 代码块，每一个子类都有相应的工厂类，抽象工厂类基于输入的工厂类返回相应的子类。

刚开始，你可能会感到困惑；但是一旦看到它的实现，就会很容易掌握和明白它和工厂模式细微的不同，我们将使用同工厂模式中相同的超类和子类：

# 抽象工厂模式超类和子类

`Computer.java`

```java

package com.journaldev.design.model;
 
public abstract class Computer {
     
    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();
     
    @Override
    public String toString(){
        return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
    }
}

```

`PC.java`

```java

package com.journaldev.design.model;
 
public class PC extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public PC(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}

```

`Server.java`

```java

package com.journaldev.design.model;
 
 
public class Server extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public Server(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}

```

# 每个子类的工厂类

首先需要创建一个抽象工厂的接口或抽象类。

`ComputerAbstractFactory.java`

```java

package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;

public interface ComputerAbstractFactory {

	public Computer createComputer();

}

```

注意: `createComputer()` 方法返回超类 `Computer` 的实例。

下面，我们的工厂类将实现这个接口并返回各自的子类对象：

`PCFactory.java`

```java

package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;
import com.journaldev.design.model.PC;

public class PCFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public PCFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public Computer createComputer() {
		return new PC(ram,hdd,cpu);
	}

}

```

同样，有一个 `Server` 子类的工厂类：

`ServerFactory.java`

```java

package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;
import com.journaldev.design.model.Server;

public class ServerFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public ServerFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	
	@Override
	public Computer createComputer() {
		return new Server(ram,hdd,cpu);
	}

}

```

下面，创建一个消费者类，为客户端类提供创建子类的入口：

`ComputerFactory.java`

```java

package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;

public class ComputerFactory {

	public static Computer getComputer(ComputerAbstractFactory factory){
		return factory.createComputer();
	}
}

```

注意：这是个很简单的类，`getComputer` 方法接受一个 `ComputerAbstractFactory` 参数，然后返回 `Computer` 对象；这点必须要清楚。

下面是一个简单的测试方法，如何使用抽象工厂获取子类的实例：

`TestDesignPatterns.java`

```java

package com.journaldev.design.test;

import com.journaldev.design.abstractfactory.PCFactory;
import com.journaldev.design.abstractfactory.ServerFactory;
import com.journaldev.design.factory.ComputerFactory;
import com.journaldev.design.model.Computer;

public class TestDesignPatterns {

	public static void main(String[] args) {
		testAbstractFactory();
	}

	private static void testAbstractFactory() {
		Computer pc = com.journaldev.design.abstractfactory.ComputerFactory.getComputer(new PCFactory("2 GB","500 GB","2.4 GHz"));
		Computer server = com.journaldev.design.abstractfactory.ComputerFactory.getComputer(new ServerFactory("16 GB","1 TB","2.9 GHz"));
		System.out.println("AbstractFactory PC Config::"+pc);
		System.out.println("AbstractFactory Server Config::"+server);
	}
}

```

输出结果:

```java

AbstractFactory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
AbstractFactory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz

```

下面是抽象工厂设计模式实现的类图：

![抽象工厂模式]({{ site.url }}/assets/images/posts/abstract-factory-pattern.png "抽象工厂模式")

# 抽象工厂设计模式的优点

- 抽象工厂模式提供面向接口编程而不是面向实现。

- 抽象工厂是"工厂的工厂"，易于扩展以适用更多产品；例如，我们可以再添加一个子类 `Laptop` 和它的工厂类 `LaptopFactory`。

- 抽象工厂模式更加健壮，避免了工厂模式中的条件逻辑。

# JDK中的抽象工厂设计模式例子

- `javax.xml.parsers.DocumentBuilderFactory#newInstance()`
- `javax.xml.transform.TransformerFactory#newInstance()`
- `javax.xml.xpath.XPathFactory#newInstance()`



你可以从 [GitHub](https://github.com/journaldev/journaldev/tree/master/java-design-patterns/Abstract-Factory-Design-Pattern) 上下载示例代码

---

原文: [Abstract Factory Design Pattern in Java](https://www.journaldev.com/1418/abstract-factory-design-pattern-in-java)