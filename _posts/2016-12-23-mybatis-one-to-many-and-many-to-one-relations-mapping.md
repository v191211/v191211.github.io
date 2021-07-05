---
layout: post
title: "Mybatis 一对多和多对一关系映射配置"
categories: ["技术"]
tags: ["Mybatis"]
---

* 目录
{:toc .toc}
## 一对多

例：一个顾客对应多个订单。

```xml
<resultMap id="customerMap" type="com.github.mybatis.Customer">
	<id property="id" column="id"/>
	<result property="name" column="name"/>
	<collection property="orders" ofType="com.github.mybatis.Order">
		<id property="id" column="o_id"/>
		<result property="name" column="o_name"/>
	</collection>
</resultMap>
```



## 多对一

例：多个订单属于一个顾客。

```xml
<resultMap id="orderMap" type="com.github.mybatis.Order">
	<id property="id" column="o_id"/>
	<result property="name" column="o_name"/>
	<association property="customer" javaType="com.github.mybatis.Customer">
		<id property="id" column="id"/>
		<result property="name" column="name"/>
	</association>
</resultMap>
```
