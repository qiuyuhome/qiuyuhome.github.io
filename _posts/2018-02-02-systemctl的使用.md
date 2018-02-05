---
layout: post
title: systemctl的使用
categories: Linux
description: systemctl的使用
keywords: Linux
---

## 在项目中, 总会用到各种的服务. 如果这个服务没有封装, 我一般都会用`nohug`来后台运行.
直到前几天查看一些资料, 发现可以自己写一个配置文件, 就可以使用`systemctl`来控制服务的运行了.
所以, 我又有事干了.

## 参考

[Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

