---
layout: post
title: 在CentOS7环境下安装chrome浏览器
categories: 在CentOS7环境下安装chrome浏览器
description: 在CentOS7环境下安装chrome浏览器
keywords: linux
---


# 在CentOS 7环境下安装chrome浏览器

文章来源: http://www.cnblogs.com/fengbohello/p/4871445.html

### 修改yum源

在/etc/yum.repos.d/目录下新建文件google-chrome.repo，向其中添加如下内容:

```bash
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

### 安装

sudo yum install google-chrome-stable
注：Google官方源在国内可能无法正常访问，导致安装失败或安装后无法正常更新，可以尝试添加--nogpgcheck参数再安装。

```
sudo yum install google-chrome-stable --nogpgcheck
```

或者可以尝试修改gpgcheck=0再安装。

