---
layout: post
title: "Mysql CTE 列数据过长异常"
categories: ["技术"]
tags: ["Mysql", "CTE"]
---

* 目录
{:toc .toc}


## 异常：Mysql CTE Data too long for column 'str' at row 1...[^1]

对于该异常 Mysql 官网上描述的很详细：

在严格（strict）SQL 模式下，语句会产生一个错误：

```mysql
ERROR 1406 (22001): Data too long for column 'str' at row 1
```

因为要对该错误进行处理，所以语句不会执行长度截断的操作。

在非递归的 SELECT 部分使用 CAST() 方法增加 str 列的长度。

```mysql
WITH RECURSIVE cte AS
(
  SELECT 1 AS n, CAST('abc' AS CHAR(20)) AS str
  UNION ALL
  SELECT n + 1, CONCAT(str, str) FROM cte WHERE n < 3
)
SELECT * FROM cte;
```

[^1]: [Mysql8 参考手册](https://dev.mysql.com/doc/refman/8.0/en/with.html){:target="_blank"}

