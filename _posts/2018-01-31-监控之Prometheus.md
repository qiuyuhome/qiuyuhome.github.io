---
layout: post
title: 监控之Prometheus
categories: 计算机
description: 监控之Prometheus
keywords: Prometheus, 监控, 系统
---

对于服务器的监控, 可以说是必不可少的. 很早之前, 公司老大就告诉我说公司里别的团队用了一个很不错的监控系统. 一直忙于开发, 也就没研究. 这几天就研究了一下.
确实是一个很NB的架构. 同时也遇到了很多新的技术. 还有很多坑. `有坑不怕, 要早点跳进去`.

## Refer

[Prometheus](https://prometheus.io/)

[Grafana](https://grafana.com/)

[Prometheus 实战](https://www.gitbook.com/book/songjiayang/prometheus/details)

[Prometheus入门](http://www.10tiao.com/html/357/201705/2247485232/1.html)

## Install

安装就不说什么了. 官网手册很详细, 也后很多的例子.

## Prometheus的理解

想要正确的使用, 就需要了解它的实现方式.

### 首先要理解几个名词:

#### time series

    中文叫`时间序列`, 是一组按照时间发生先后顺序进行排列的数据点序列。
    通常一组时间序列的时间间隔为一恒定值（如1秒，5分钟，12小时，7天，1年），因此时间序列可以作为离散时间数据进行分析处理。
    时间序列广泛应用于数理统计、信号处理、模式识别、计量经济学、数学金融、天气预报、地震预测、脑电图、控制工程、航空学、通信工程以及绝大多数涉及到时间数据测量的应用科学与工程学。

#### TSDB

    TSDB(Time Series Database)时序列数据库，我们可以简单的理解为一个优化后用来处理时间序列数据的软件，并且数据中的数组是由时间进行索引的。

#### 时间序列数据库的特点

    大部分时间都是写入操作。
    
    写入操作几乎是顺序添加，大多数时候数据到达后都以时间排序。
    
    写操作很少写入很久之前的数据，也很少更新数据。大多数情况在数据被采集到数秒或者数分钟后就会被写入数据库。
    
    删除操作一般为区块删除，选定开始的历史时间并指定后续的区块。很少单独删除某个时间或者分开的随机时间的数据。
    
    基本数据大，一般超过内存大小。一般选取的只是其一小部分且没有规律，缓存几乎不起任何作用。
    
    读操作是十分典型的升序或者降序的顺序读。
    
    高并发的读操作十分常见。

#### 常见的时间序列数据库

| TSDB项目    | 官网                            |
|:-----------|:-------------------------------|
| influxDB   | https://influxdata.com/        |
| RRDtool    | http://oss.oetiker.ch/rrdtool/ |
| Graphite   | http://graphiteapp.org/        |
| OpenTSDB   | http://opentsdb.net/           |
| Kdb+       | http://kx.com/                 |
| Druid      | http://druid.io/               |
| KairosDB   | http://kairosdb.github.io/     |
| Prometheus | https://prometheus.io/         |

#### 什么是Prometheus

    Prometheus是由SoundCloud开发的开源监控报警系统和时序列数据库(TSDB)。Prometheus使用Go语言开发，是Google BorgMon监控系统的开源版本。
    
    2016年由Google发起Linux基金会旗下的原生云基金会(Cloud Native Computing Foundation), 将Prometheus纳入其下第二大开源项目。Prometheus目前在开源社区相当活跃。
    
    Prometheus和Heapster(Heapster是K8S的一个子项目，用于获取集群的性能数据。)相比功能更完善、更全面。Prometheus性能也足够支撑上万台规模的集群。

#### Prometheus的特点

    多维度数据模型。
    
    灵活的查询语言。
    
    不依赖分布式存储，单个服务器节点是自主的。
    
    通过基于HTTP的pull方式采集时序数据。
    
    可以通过中间网关进行时序列数据推送。
    
    通过服务发现或者静态配置来发现目标服务对象。
    
    支持多种多样的图表和界面展示，比如Grafana等。

#### Prometheus相关组件

Prometheus生态系统由多个组件组成，它们中的一些是可选的。多数Prometheus组件是Go语言写的，这使得这些组件很容易编译和部署。

* Prometheus Server

    主要负责数据采集和存储，提供PromQL查询语言的支持。

* 客户端SDK

    官方提供的客户端类库有go、java、scala、python、ruby，其他还有很多第三方开发的类库，支持nodejs、php、erlang等。

* Push Gateway

    支持临时性Job主动推送指标的中间网关。

* PromDash

    使用Rails开发可视化的Dashboard，用于可视化指标数据。

* Exporter

    Exporter是Prometheus的一类数据采集组件的总称。它负责从目标处搜集数据，并将其转化为Prometheus支持的格式。与传统的数据采集组件不同的是，它并不向中央服务器发送数据，而是等待中央服务器主动前来抓取。

    Prometheus提供多种类型的Exporter用于采集各种不同服务的运行状态。目前支持的有数据库、硬件、消息中间件、存储系统、HTTP服务器、JMX等。

* alertmanager

    警告管理器，用来进行报警。

* prometheus_cli

    命令行工具。

* 其他辅助性工具

    多种导出工具，可以支持Prometheus存储数据转化为HAProxy、StatsD、Graphite等工具所需要的数据存储格式。

#### Prometheus的架构

下面这张图说明了Prometheus的整体架构，以及生态中的一些组件作用:

![prometheus](/images/posts/prometheus.png)

Prometheus的基本原理是通过HTTP协议周期性抓取被监控组件的状态，任意组件只要提供对应的HTTP接口就可以接入监控。不需要任何SDK或者其他的集成过程。这样做非常适合做虚拟化环境监控系统，比如VM、Docker、Kubernetes等。输出被监控组件信息的HTTP接口被叫做exporter 。目前互联网公司常用的组件大部分都有exporter可以直接使用，比如Varnish、Haproxy、Nginx、MySQL、Linux系统信息(包括磁盘、内存、CPU、网络等等)。

> 我的理解: 开放一个通过http协议能够访问到的url(如: prometheus.qiuyuhome.com), 把搜集到的信息, 都在这个url中展示出来. 在`Prometheus`的配置中加入对这个url的拉取, 就可以了.

**Prometheus服务过程大概是这样：**

* Prometheus Daemon负责定时去目标上抓取metrics(指标)数据，每个抓取目标需要暴露一个http服务的接口给它定时抓取。Prometheus支持通过配置文件、文本文件、Zookeeper、Consul、DNS SRV Lookup等方式指定抓取目标。Prometheus采用PULL的方式进行监控，即服务器可以直接通过目标PULL数据或者间接地通过中间网关来Push数据。
* Prometheus在本地存储抓取的所有数据，并通过一定规则进行清理和整理数据，并把得到的结果存储到新的时间序列中。
* Prometheus通过PromQL和其他API可视化地展示收集的数据。Prometheus支持很多方式的图表可视化，例如Grafana、自带的Promdash以及自身提供的模版引擎等等。Prometheus还提供HTTP API的查询方式，自定义所需要的输出。
* PushGateway支持Client主动推送metrics到PushGateway，而Prometheus只是定时去Gateway上抓取数据。
* Alertmanager是独立于Prometheus的一个组件，可以支持Prometheus的查询语句，提供十分灵活的报警方式。

#### Prometheus适用的场景

Prometheus在记录纯数字时间序列方面表现非常好。它既适用于面向服务器等硬件指标的监控，也适用于高动态的面向服务架构的监控。对于现在流行的微服务，Prometheus的多维度数据收集和数据筛选查询语言也是非常的强大。Prometheus是为服务的可靠性而设计的，当服务出现故障时，它可以使你快速定位和诊断问题。它的搭建过程对硬件和服务没有很强的依赖关系。

#### Prometheus不适用的场景

Prometheus它的价值在于可靠性，甚至在很恶劣的环境下，你都可以随时访问它和查看系统服务各种指标的统计信息。 如果你对统计数据需要100%的精确，它并不适用，例如：它不适用于实时计费系统。



