---
layout: post
title: MySQL中, 字段 = 字符串 + 字段, 处理方法
categories: SQL
description: MySQL中, 字段 = 字符串 + 字段, 处理方法
keywords: MySQL
---

# MySQL中, 字段 = 字符串 + 字段, 处理方法

今天在工作中, 遇到一个问题. 如下

表中有一个字段, 是多个课程ID, 用英文的都好分隔, 导出的CSV, 会出现科学计数法. 
所以, 我想在这个表中, 每天数据的这个字段, 最前面, 都加上一个反引号, 那么, SQL语句怎么写呢?

我的第一个想法就是: 

```
update tablename set field = '`' + field;
```

可惜这样是不行的. 查找资料. 正确的方法如下:

```
update tablename set field = concat('`', field);
```






