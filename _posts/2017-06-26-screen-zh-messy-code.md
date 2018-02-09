---
layout: post
title: screen下. 中文乱码问题解决方法
categories: Linux
description: screen下. 中文乱码问题解决方法
keywords: Screen, Linux
---

screen下. 中文乱码问题解决方法

```bash
vi ~/.screenrc
```


```bash
# 新增下面2行
defencoding = UTF-8
encoding = UTF-8
```

保存的日志, 用vim打开, 也是乱码. 

在vim中输入

```bash
:set encoding=utf-8

```

    

