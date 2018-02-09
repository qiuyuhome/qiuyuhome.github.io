---
layout: post
title: CentOS下载jdk
categories: Linux
description: CentOS下载jdk
keywords: CentOS, jdk, java
---

在CentOS下, 要下载jdk, 但是复制官网的下载地址, 用wget下载下来的是一个网页. 在网上找到了一个解决把饭. 

参考: https://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/#

```
# cd /opt/
# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz"

# tar xzf jdk-8u121-linux-x64.tar.gz
```

下载后, 解压, 然后把java加入环境变量, 就ok了. 



