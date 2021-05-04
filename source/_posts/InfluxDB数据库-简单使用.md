---
title: 【InfluxDB数据库】简单使用
date: 2020-08-01 09:26:34
categories:
	- Data Analysis
tags:
	- software
	- influxdb
---

版本：V2.0-Beta

## 1. 一些概念

### Line protocol

influx使用line protocol来写入数据，格式和v1.8没有区别，如下：

<!--more-->

```
// Syntax ，
<measurement>[,<tag_key>=<tag_value>[,<tag_key>=<tag_value>]] <field_key>=<field_value>[,<field_key>=<field_value>] [<timestamp>]

// Example
myMeasurement,tag1=value1,tag2=value2 fieldKey="fieldValue" 1556813561098000000
```

注意：

- 不能使用`\n`换行
- 浮点数可以使用科学计数法
- 整数结尾要加一个`i`
- 无符号整数结尾加一个`u`
- 字符串长度限制在 64KB
- 一般情况下使用的是unix时间戳
- 需要转义的字符不多：空格，逗号，等号，引号，反斜杠等，使用`\`进行转义
- 注释：`#`
- 命名不能以`_`开头

### 带有注释的CSV
文件`data.csv`

## 2. 写入数据的方法

### 从用户界面写入【最直观】

1. influxDB 2.0 OSS界面中，选择**Data(load Data) > Buckets**
2. 在希望写入数据的Bucket下选择**+Add Data**
3. 然后会给三个方法：
   （1）使用line protocol，上传文件（文件格式应为.line），或者手动输入；
     （2）**爬虫方法——Scrape Metrics**，但是这个爬虫/刮板方法似乎很鸡肋，其爬取后的数据到底表示什么？可以点击**Explore > submit**查看数据情况，但是数据表示什么仍然不确定。
   其格式为Prometheus，值得研究一下。
     （3）**Telegraf**插件。

### CLI写入

用命令行方法，但是实现效果不佳

使用命令`influx write -o -b -p -t -f`
分别是org，bucket，precision，token，file

- 可以用`\`换行书写
- 可以不用上传file的方式，直接写数据

### 使用InfluxDB API写入【curl http】

- 为什么要用这个写，主要是通过http方式将数据写入。
- 写入数据压缩或者不压缩在代码上是有区别的。