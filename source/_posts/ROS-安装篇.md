---
title: 【ROS】安装篇
date: 2020-07-28 10:00:41
categories:
	- ROS
tags:
	- ROS
	- software
---

安装一波三折值得细品

## 0.系统

使用Ubuntu原生系统自然最好；

退而求其次，可以使用虚拟机的ubuntu系统，也很好用；

曾尝试在Windows linux subsystem 上安装ubuntu和ROS，可以实现安装，也可以简单使用，但是针对仿真等比较耗内存的应用体验会比较差，而且还需要使用Xserver实现桌面映射。

当前时间，最好用的是ubuntu 18.04系统，对应ROS-melodic版本，内容相对完善，也比较新；

ubuntu 20.04系统对应的ROS-Noetic在一些常用包上还是存在缺失问题，后期使用中会出现各种不兼容问题，值得注意。

<!--more-->

## 1. 换源

ROS系统需要的安装包大多来自 ros.org 或 github，建议使用换源以提高安装的成功率和效率；经常下载一小时，报错马上崩，很影响心态。

亲测比较好用的apt换源有：

[[简书换阿里源](https://www.jianshu.com/p/ad4dc5dbf55e)]

pip换源：

[[更换linux系统中的pip源](https://www.cnblogs.com/g15009428458/p/12323137.html)]

以及中科大源，见本文末。

## 2. 安装

[[ROS安装文章非常多](https://blog.csdn.net/sinat_38284212/article/details/100884644)]，基本步骤相同。

一定要按部就班地来：

最容易出现的问题是：`unable to load package xxx`

原因：

（1）换源失败

（2）版本错误

（3）没有更新软件和源

*参考：[Ubuntu18.04.1安装ROS（'E:无法定位软件包'）](https://blog.csdn.net/sinat_34130812/article/details/81666728)*



1. 参考[官网](http://wiki.ros.org/ROS/Installation/UbuntuMirrors)

```bash
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
```

获取镜像

2. 设置密钥

```bash
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

如果不设置，就会出现问题：[sudo apt-get update出现W: GPG 错误：http://packages.ros.org/ros/ubuntu xenial InRelease: 由于没有公钥，无法验证下列签名](https://blog.csdn.net/wangxue_1231/article/details/92801510)

3. 重新更新一下（一定要！）

```bash
sudo apt-get update
```

4. 正式安装

```bash
sudo apt install ros-melodic-desktop-full
```

5. 启动：

```bash
sudo rosdep init
```

失败！

最后容易出现的问题类似：

[ubuntu16.04 运行rosdep init 返回ERROR：cannot download default sources list from](https://blog.csdn.net/qq_35590091/article/details/100515558)

其本质原因还是换源没有换好，如果

上面这个解决方法也不是太好，不妨使用下面文章中方法：

[ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/ma](https://blog.csdn.net/weixin_43288910/article/details/105627358?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3)

也就是换成中国科大的源，亲测可用，即可愉快玩耍了！

## rosdep 问题

rosdep是ROS中推荐的一种安装方法，比如使用开源的机器人包，rosdep可以很方便地将package中需要的依赖找出并全部安装，听起来确实非常棒。

但是实测会有一些小问题：

- rosdep 无法使用的问题，非常普遍，网上建议一般是没有安装好rosdep，进行安装；
- 安装rosdep之后，ros其他命令失效，这个问题实在是遇到了多次（不停地想尝试一下），最终还是选择重装ROS，其实不使用rosdep也不是大问题，在后续使用过程中逐步添加ROS的依赖包，也是可以的。



