---
title: 【InfluxDB数据库】入门
date: 2020-07-31 21:28:23
categories: 
	- Data Analysis
tags:
	- software
	- influxdb
---

版本：v2.0-beta
系统：win+docker / ubuntu服务器

## 1. Docker 安装
[Docker-desktop官方下载链接](https://www.docker.com/products/docker-desktop)
安装完成之后，就可以直接在终端运行docker了
或者可以在VSCode中使用docker插件

## 2. VSCode 中 Docker 插件探索

<!--more-->

## 3. 在Docker中使用influxdb
以下两句代码只需使用任意一句即可。

```
#下载页面的 docker image
docker pull quay.io/influxdb/influxdb:2.0.0-beta

#介绍页面的安装方法
docker run --name influxdb -p 9999:9999 quay.io/influxdb/influxdb:2.0.0-beta
```

安装完成后，会显示：

然后在浏览器中访问`localhost:9999`即可打开v2.0界面。

## 4. 服务器安装V2.0
### 4.1 首先将压缩包上传到服务器
(1)使用代码，但是不好用

```
scp -r localfile.txt username@192.168.0.1:/home/username/
```
其中，
１）scp是命令，-r是参数
２）localfile.txt 是文件的路径和文件名
３）username是服务器账号
４）192.168.0.1是要上传的服务器ip地址
５）/home/username/是要拷入的文件夹路径

（2）直接用filezilla、MobaXterm等工具上传到服务器
### 4.2 安装
输入下列语句，主要是打开压缩包，以及安装。

```
# Unpackage contents to the current working directory
tar xvzf path/to/influxdb_2.0.0-beta.10_linux_amd64.tar.gz

# Copy the influx and influxd binary to your $PATH
sudo cp influxdb_2.0.0-beta.10_linux_amd64/{influx,influxd} /usr/local/bin/
```

### 4.3 运行
理论上输入如下语句即可运行：

`influxd`

但是同样需要访问`localhost:9999`来访问：

（1）首先这里的`localhost`指的是服务器的地址；

（2）服务器（尤其是云服务器）需要注意添加9999安全组的IP路径，否则是无法访问的。

修改成功后，登陆`ip_of_ECS:9999`就可以登陆V2.0界面了！
![v2.png](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/1240.png)
Beautiful！

最后，提示一下**如果安装了V2.0版本后，原来的V1.8版本的命令就失效了**，以后想使用influxDB仅仅输入`influx`是不行的了！