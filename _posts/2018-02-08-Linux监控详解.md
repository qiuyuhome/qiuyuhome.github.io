---
layout: post
title: Linux监控详解
categories: [Linux, 计算机]
description: Linux监控详解
keywords: Linux
---

## Load Average

`top` 命令会看到这个值.

有3个值, 分别是1分钟, 5分钟, 15分钟的平均值.

load average是基于内核的数量决定的，可以简单的理解为每个内核load之和。

按每个内核100%负载来算，4个内核，load average的值就是4。

那到底多高的负载是一个临界点呢？这没有一个绝对值，一个比较被人所接受的说法是： load average 应该 <= cpu核数 * 0.7




