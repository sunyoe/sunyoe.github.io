---
title: PyTorch-学习篇2
date: 2020-11-25 14:55:56
categories:
	- 机器学习
tags:
	- PyTorch
---

> 本篇主要对学习中常见的torch、tensor方法进行记录，不定期更新。

---

# 模块 package

`torch.util.data`包，参考：[pytorch实现自由的数据读取－torch.utils.data的学习](https://blog.csdn.net/tsq292978891/article/details/79414512)

- `Dataset`

- `Dataloader`
- 用于读取数据集

`torch.nn.DataParallel` 可以使用多个GPU来加速训练

`torch.distributed`也是一个和分布式训练，多GPU训练有关的包

`torch.optim`是一个思想了各种优化算法的库，可以用用于构建optimizar，里面有比较熟悉的SGD、Adam等优化器

- 包括`optimizer.zero_grad()`，`loss.backward()`，`optimizer.step()`

<!--more-->

---

# 方法 function

一维list转化为三维list：`mean[:, None, None]`（前提是mean的类型是tensor）

增加维度：`image[None]`（前提是tensor）

生成序列：`torch.arange(start=1,end=6)` 里面会有两个参数，关键词可以不用写

- `torch.range`的结果会包含 end
- `torch.arange`的结果不包含 end
- 两者创建的tensor类型也不一样，参考：[pytorch.range() 和 pytorch.arange() 的区别](https://blog.csdn.net/m0_37586991/article/details/88830026)

**传入行坐标、列坐标，生成网格行坐标矩阵的列坐标矩阵**：`torch.meshgrid`【这个真的是有点惊艳】

- `x1, y1 = torch.meshgrid(x, y)`
- 传入的 x，y 是两个坐标轴
- 输出的 x1, y1 是上面的x，y形成的网格的每一个角点的横纵坐标

**拼接：**

- `torch.cat((A, B), 0)` 在指定维度(0)进行拼接
- `torch.stack((T1, T2), 0)`会在指定维度位置**新增一个维度**，然后进行拼接
  - 该函数要求输入的**两个待拼接序列的shape必须是一样的**

`tensor.detach()`准确来说是pytorch中的Variable对象的方法

- 官方介绍：Returns a new Tensor, detached from the current graph.
  The result will never require gradient.
- 返回一个新的从当前图中分离的variable，这个返回的variable将不会需要梯度
- 参考：[pytorch中的detach和detach_](https://www.cnblogs.com/jiangkejie/p/9981707.html)

`torch.numel(x) `返回数组中元素的数目

`torch.unbind(x, dim=1)`函数：移除指定维后，返回元组包含沿着指定维切片后的各个切片，参考：[PyTorch 函数解释：torch.narrow()、torch.unbind()](https://blog.csdn.net/dreamhome_s/article/details/106032025)

`torch.topk`沿给定维度返回输入张量input中k个最大值，也可以返回最小的k个值（需要指定largest=False）

`tensor.clean`

展平，或者说也是一种变形：

- `tensor.view()`
  - 【只针对连续内存，如果索引顺序变化，自然也就不再连续了】
- `tensor.reshape()`
  - 没有连续内存的限制
  - 里面如果有 -1，表示这一位会根据其他参数计算而得出来【毕竟总数是知道的，其他维上的数量知道了，这一维也就确定了】
- `flatten`

**使用tensor可以实现在矩阵中找小于大于某数的数的索引值的比较快的方式**

```python
# 蒙版索引
between_thresholds=(matched_vals>=self.low_threshold)&(matched_vals<self.high_threshold)
# 直接使用，从而找出matched_vals中介于满足两个阈值要求的数值
matrix[between_thresholds]
```

`torch.randperm(n)` 把0到n-1这些数随机打乱，得到一个数字序列

`torch.clamp(input, min, max)` 将输入的input张量的每个元素夹紧到[min,max]区间，返回新张量。也就是说，大于max值的会缩小为max，小于min值得会放大为min值。

`torch.nonzero(input)` 输入应为tensor类型，输出为两个tensor，记录了input中非零元素的索引位置（用横纵坐标来理解更容易）

