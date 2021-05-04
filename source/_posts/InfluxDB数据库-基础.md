---
title: 【InfluxDB数据库】基础
date: 2020-07-30 09:31:44
categories:
	- Data Analysis
tags:
	- software
	- influxdb
---

系统：ubuntu
influx版本：v1.8

## 1. 安装

直接按照网站方法即可实现安装：  
[install介绍](http://docs.influxdata.com/influxdb/v1.8/introduction/install/)

<!--more-->

## 2. 数据写入

### 2.1 创建和使用databases

`> create database mydb`
关键词大小写均可，但是文件上使用的是大写；
`> show databases`
可以展示所有数据库；
`> use mydb`
使用数据库`mydb`，可以定向使用某个数据库

### 2.2 写入数据

>哪些数据可以被写入？

#### 点的概念 - points

点由`time`、`measurement`、`field`、`tags`构成：
其中，`measurement`视为SQL表，时间是基础索引值（？）  
tags和fields是SQL表中的有效列；  
tags被索引，而fields不被索引；  
**<font color=red>使用INSERT进行点数据的输入</font>**
`id,tags=属性 空格 value=数值`
每一部分内部用“，”分割，相互之间用空格分割，例如：
`> insert temperature,machine=unit42,type=assembly external=25,internal=37`

- **时间戳 - time**
  参见v2.0版本的介绍；
  因为本数据库用的是“时间序列”
  timestamp

- **`measurement`其实就是`id`**
  例如`cpu_load`?
  到底是什么

- **`field`**
  要测量的value，比如
  `value=0.64` `temperature=21.2`

- **`tags`**
  `tags`是和value有关的所有数据(metadata)
  例如host、region、dc等，**可是这些都是什么？**

> **InfluxDB line protocol**
> 需要使用该工具写入InfluxDB
> [参考页面](https://docs.influxdata.com/influxdb/v1.8/write_protocols/line_protocol_reference/#line-protocol-syntax)

## 3. 搜索数据 - Query - 询问

1. 检索指定的数据内容：
   `> select "value_or_tag" from  "id/measurement"`
   <font color=red>一定要注意`from`前后都有空格！</font>
2. 检索某个`id`的所有内容
   `> select * from "id/measurement"`
   但是文档上说，用这个不是太好，如果信息过多的话可能会有问题。
3. `> select * from /.*/ limit 1`
   这个没有解释，但从效果来看，是把所有数据库里面的信息都展示出来了。
4. `> select * from "id/measurement" where "value" > 0.9`
   添加选择性语句，用于选择满足条件的数据。
5. 更多的方法
   参考：[features and keywords](https://docs.influxdata.com/influxdb/v1.8/query_language/spec/)