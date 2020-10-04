---
layout: post
title: 进程
categories: 操作系统
description: 计算机操作系统之进程
keywords: process, 进程
typora-root-url: "/Users/qiuyu/code/blog/qiuyuhome.github.io"
---

# 文件描述符



## 一. 概念

在 linux 系统中, **一切皆文件**, 当进程打开现有文件或创建新文件时，内核向进程返回一个文件描述符，文件描述符就是内核为了高效管理已被打开的文件所创建的索引，用来指向被打开的文件，所有执行I/O操作的系统调用都会通过文件描述符.





