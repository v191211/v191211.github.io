---
layout: post
title: "Mybatis 批量插入数据到 Oracle 数据库"
categories: ["技术"]
tags: ["Mybatis"]
---

* 目录
{:toc .toc}
## Oracle 批量插入数据 SQL

```sql
INSERT INTO table_name (column1, column2, column3)
  (SELECT ?, ?, ? FROM dual
     UNIONALL
   SELECT ?, ?, ? FROM dual )
```



## XML SQL 片段

```xml
<insert id="insertSelective" parameterType="ArrayList" useGeneratedKeys="true">
  <selectKey resultType="int" keyProperty="seqNo" order="BEFORE">
    SELECT MAG_CUSTOMER_BGY_SD_SEQ.NEXTVAL FROM DUAL
  </selectKey>
    INSERT INTO MAG_CUSTOMER_BGY_SD
    (SEQNO,
     ENTERPRISECODE,
     CUSTOMERCODE,
     CUSTOMERTYPE,
     PERIODNO,
     OPERTYPE
    ) SELECT MAG_CUSTOMER_BGY_SD_SEQ.NEXTVAL, SD.* FROM (
    <foreach collection="list" item="item" index="index" separator="UNIONALL">
      select
        #{item.enterpriseCode, jdbcType=VARCHAR},
        #{item.customerCode, jdbcType=VARCHAR},
        #{item.customerType, jdbcType=VARCHAR},
        #{item.periodNo, jdbcType=VARCHAR},
        #{item.operType, jdbcType=VARCHAR}} from dual
    </foreach>
    ) SD
  </insert>
```

keyProperty：Javabean 字段。

返回的 int 结果是插入的数据条数，不是序列号。