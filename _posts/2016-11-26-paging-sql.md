---
layout: post
title: "Oracle 和 Mysql 中的分页 SQL"
categories: ["技术"]
tags: ["SQL"]
---

* 目录
{:toc .toc}
## Mysql 分页 SQL

```
SELECT * FROM table LIMIT m,n;
```
m：记录开始索引位置。

n：取多少条记录  。



## Oracle 分页 SQL

```
SELECT * FROM 
(
SELECT ROWNUM row,t.* FROM
(SELECT * FROM table) t WHERE row<=10
)
WHERE row >= 1
```
1：起始索引位置。

10：结束索引位置。