---
layout: post
title:  CentOS 网络配置
categories: Linux
description: CentOS 网络配置
keywords: Linux, CentOS, Network
---

CentOS 网络配置, 主要针对的是CentOS7以前的版本. CentOS7以后的版本命令有些差别, 但不是很大. 

# CentOS 网络配置
## 查看IP


命令: `ifconfig`

## 临时修改IP, 网络掩码, 网关


命令: `fconfig eth0` 
（默认是第一个网卡） 后面接IP地址， 网络掩码和 网关
如果不设置，就使用默认的掩码


## 修改IP配置

```
vim /etc/sysconfig/network-scripts/ifcfg-eth0
```
	
```
DEVICE=eth0
BOOTPROTO=none
NM_CONTROLLED=no
ONBOOT=yes
HWADDR=00:0c:29:e4:4b:5b
ONBOOT=yes
IPADDR=192.168.1.84
NETMASK=255.255.255.0
TYPE=Ethernet
UUID="ec1d0b85-1891-4600-952a-cfcf6ae0c789"
GATEWAY=192.168.1.4
DNS1=114.114.114.114
IPV6INIT=no
USERCTL=no
```

## 配置详解:
	
```
DEVICE=eth0
	网卡对应的设备别名，如ifcfg-eth0第一块网卡
BOOTPROTO=none
	设置网卡获得ip地址的方式，选项可以为为static，dhcp或bootp
NM_CONTROLLED=no

ONBOOT=yes
	系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备。默认设置为yes
HWADDR=00:0c:29:e4:4b:5b
	对应的网卡物理地址
IPADDR=192.168.1.84
	只有网卡设置成static时，才需要此字段
NETMASK=255.255.255.0
	网卡对应的网络掩码
TYPE=Ethernet

UUID="ec1d0b85-1891-4600-952a-cfcf6ae0c789"

GATEWAY=192.168.1.4

DNS1=114.114.114.114

IPV6INIT=no
	是否启用IPV6
USERCTL=no
	是否允许非root用户控制该设备，设置为no，只能用root用户更改
```


## 配置完IP后, 需要重启网卡

命令: `service network restart`




## CentOS修改网关
**临时修改网关** : 
`route add default gw 192.168.1.1 dev eth0`

**永久修改网关** : 
`vi  /etc/sysconfig/network`
`NETWORKING=yes` 		#表示系统是否使用网络，一般设置为yes。如果设为no，则不能使用网络。
`HOSTNAME=centos` 	#设置本机的主机名，这里设置的主机名要和/etc/hosts中设置的主机名对应
`GATEWAY=192.168.1.1` #设置本机连接的网关的IP地址。
##修改网关后要重启网卡
命令: `service network restart`


## CentOS修改DNS

```
vi /etc/resolv.conf
```

```
nameserver 114.114.114.114
nameserver 8.8.8.8
```

如果centos系统建立在虚拟机之上，那么在设置虚拟机的网络时请选择‘网桥适配器’连接


## centos 图形配置网络
命令: `setup`






