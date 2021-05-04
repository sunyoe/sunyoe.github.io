---
title: PyTorch-学习篇1
date: 2020-11-24 21:38:26
categories:
	- 机器学习
tags:
	- PyTorch
---

> 本文根据PyTorch官方推荐同名书籍：《Deep learning with pytorch》的个人学习笔记而来。

# Deep learning with pytorch

基于优化任务性能的算法的基础上，自动提取分层特征的算法

相对于手动选择特征，会有什么样的不同，以及怎么样去实现ta？1.3节涉及

To train this model, you need a few things (besides the loop itself, which can be a standard Python for loop): a source of training data, an optimizer to adapt the model to
the training data, and a way to get the model and data to the hardware that will be performing the calculations needed for training the model.  

要训练该模型，您需要做一些事情（除了循环本身，它可以是标准的Python for循环）：训练数据的来源，用于使模型适应训练数据的优化器以及获取模型的方法 并将数据发送到硬件，该硬件将执行训练模型所需的计算。

<!--more-->

## tensor

从tensor形式转化为storage形式，需要使用offset、size、stride三个变量

tensor是一维的

stride表示的是当tensor的维数+1时，元素必须跳过skip的个数

在一个二维的tensor中

$x_{i,j}=storage\_offset+stride[0]*i+stride[1]*j$

offset一般是 0，如果storage要存储一个很大的tensor的时候，可能offset会是一个正数

**关于tensor，我觉的最关键的就是storage的理解**

pytorch一个强大的地方就是tensor数据的排列和存取方法，当你使用tensor进行存取时，storage就是一个很灵活的仓库

你可以灵活地调整数据的维数，从而进行数据分析，而且还不会改变storage的分布，这确实是一个不错的创意

## 关于训练

本书的第三章对text、image等多种数据究竟应该如何传入神经网络进行了一点说明，并进行了比较基础的操作，非常具有启发性

本书的第四章首先展示了一个比较简单的线性回归网络的全部程序，并且使用了反向传播等操作：

- 通过不断的演化展示了在torch中可以利用其包含的API实现loss的反向传播和params的自动优化
- 关于params的grad优化问题，还对zero-grad和step等常用方法进行了一点介绍

第四章的后半部分（4.2.2以后）这是对训练过程中的一些细节问题进行讨论

包括`optimizer.zero_grad()`，`loss.backward()`，`optimizer.step()`以及autograd方法：

`params = torch.tensor([1.0, 0.0], requires_grad = True)`

参数在定义时就采用了requires_grad，那么就会自动开始autograd跟踪，使用backward时，就会将变量关于参数的导数进行自动计算了

## 一些技巧

**首先是认识到过拟合的发生**

解决方法之一是把数据集分割一下，必须要保证在每个部分都具有良好的效果才能说明模型是正确的

解决方法之二：如果你的training loss没有减小，很有可能是你的模型过于简单了

还有一种可能：你的数据没有包含足够的能够解释输出的有效信息

另一条原则：如果训练集loss和测试集loss出现了偏差，那就说明是过拟合了！

总结就是：

- 首先保证有足够的数据
- 适合的模型
  - 一种方法是增加惩罚机制从而方便地让模型更平滑、变化地更慢一点
  - 另一种方法是给输入数据增加噪声
- 最简单的方法可能是让你的模型简单一点（不用拟合所有的点，从它们中穿过就好了）

**选择合适的模型大小size，或者说参数多少的方法：**

- 首先增加size，直到能够拟合
- 然后减小，直到不再过拟合

## 开启和关闭autograd

**节省计算和存储**

其实测试集的计算根本不需要反向传播，也不需要计算导数

但是测试集的计算过程和训练集是一致的，使用了相同的函数和模型

那么为了节省导数计算和存储，可以使用`torch.no_grad`方法（训练的函数中使用的是`requires_grad`的方法，只要使用，意味就会对参数进行跟踪）

也可以使用`torch.set_grad_enabled`方法，来达到启用（enable）和关闭（disable）autograd的目的

## 使用非线性激活函数

值得注意的是，**权重 w** 可以是一个数字或者是矩阵（matrix）【这样权重w就可以掌握整个一层layer的神经元neurons】

而 **输入 x** 与**偏置 b** 必须相匹配，一般是一个数字或一个向量（vector）

激活函数的一个功能是件输出聚焦到一个指定的范围上

1. `torch.nn.Hardtanh`激活函数

小于0就全部为0，大于10就全部为10，类似这样的逻辑

2. `torch.nn.sigmoid`激活函数

3. `torch.tanh`

这些激活函数，在x逐渐为负值时，渐进地接近0或者-1；而随着x上升，逐渐接近1，但是斜率会逐渐变小

## 使用nn module搭建神经网络

 搭建`nn.Module`的子类，至少需要重新定义一个`.forward()`函数：

`.forward()`函数主要作用就是处理输入，变成输出

所以通常来说，你的整个model会是`nn.Module`的一个子类，反过来，model也可嗯那个会包含其他的`nn.Module`的子类

有三种方式实现相同的网络结构，并且使用的PyTorch功能越来越复杂

- 第一种：`nn.Sequential`

## 思维导图整理

![image-20201111104603544](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20201111104603544.png)

![Deep-Learning-with-PyTorch](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/Deep-Learning-with-PyTorch.png)

## 推荐书目

- Grokking Deep Learning, by Andrew W. Traska, is a great resource for developing
  a strong mental model and intuition on the mechanism underlying deep neural networks.
- For a thorough introduction and reference to the field, we direct you to Deep
  Learning, by Ian Goodfellow, Yoshua Bengio, and Aaron Courville.b【花书】
- Last but not least, the full version of this book is available in Early Access now, with an estimated print date in late 2019: https://www.manning.com/books/deeplearning-with-pytorch.  