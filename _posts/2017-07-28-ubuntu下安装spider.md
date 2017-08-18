---
layout: post
title: ubuntu下的装php
categories: Linux
description: ubuntu下的装php
keywords: linux
---


安装php源报错

```
add-apt-repository ppa:ondrej/php
```

解决方法:

```
正确的方法是在终端中启用UTF-8支持。

首先检查您的地点：

locale -a

然后，为en_US安装UTF-8区域设置，示例如下：

locale-gen en_US.UTF-8

那么你需要导出它：

export LANG=en_US.UTF-8

那么add-apt-repository命令就行了。

如果仍然仍然无效，请尝试使用以下行：

LC_ALL=en_US.UTF-8 add-apt-repository -y ppa:ondrej/php
```



