---
title: OpenCV常用方法
date: 2020-09-26 09:36:27
categories: 
	- [Tools]
	- [机器视觉]
tags:
	- OpenCV	
	- 华为云无人车
---

## 安装

pip install opencv-python

> 参考：[Py之cv2：cv2库(OpenCV，opencv-python)的简介、安装、使用方法(常见函数、方法等)最强详细攻略](https://blog.csdn.net/qq_41185868/article/details/79675875#%E5%AE%89%E8%A3%85OpenCV%E7%9A%84%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E6%B3%95)

注意，基于pip安装，有多种方式，上面讲的opencv-python包含主要的modules，此外还有四个opencv的版本在pip可以获得：

- [opencv-python](https://pypi.org/project/opencv-python/)包含opencv的主要modules
- [opencv-contrib-python](https://pypi.org/project/opencv-contrib-python/) 包含opencv的主要modules以及contrib modules
- [opencv-python-headless](https://pypi.org/project/opencv-python-headless/): 和opencv-python相同，但是没有GUI功能
- [opencv-contrib-python-headless](https://pypi.org/project/opencv-contrib-python-headless/):与opencv-contrib-python相同，但是没有GUI功能。

推荐在虚拟环境（conda环境）中安装第二种。

<!--more-->

此外还有安装方式：

【conda安装】

```bash
conda install -c https://conda.anaconda.org/menpo opencv3 #安装opencv3
#如果要安装opencv4将opencv3改成如下命令
conda install -c https://conda.anaconda.org/menpo opencv #安装最新版opencv4
#也可通过conda search -c https://conda.anaconda.org/menpo opencv*来搜索所有opencv版本
```

卸载：

```bash
conda unstall opencv3 #卸载opencv3
conda deactivate #退出虚拟环境
conda remove -n vm --all #删除虚拟环境conda unstall opencv3 #卸载opencv3
```

> 参考：[【opencv】1.opencv安装之使用pip或conda安装opencv](https://blog.csdn.net/u011119817/article/details/100110495)

## 图片相关

`cv2.imread()`

OpenCV定义了，图像的原点（0，0）在图片的左上角，横轴为X，朝右，纵轴为Y，朝下，如下图所示。

![image-20200902213453711](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20200902213453711.png)

需要注意的是，由于OpenCV的早期开发者习惯于使用BGR顺序的颜色模型，因此使用OpenCV的imread()读到的像素，每个像素的排列是按BGR，而不是常见的RGB，代码编写时需要注意。

`cv2.namedWindow('', cv2.WINDOW_KEEPRATIO)`

对视窗进行命名

`cv2.resizeWindow('', 0, 0)`

调整图像大小

`cv2.moveWindow('', 0, 0)`

移动视窗到合适的位置

#### 图片缩放

`cv2.resize(img, (int(y/2), int(x/2)))`

设置参数为图片的高和宽，定义后直接输出尺寸为宽和高

`cv2.resize(img, (0,0), fx=0.25, fy=0.25, interpolation=cv2.INTER_NEAREST)`

使用最近邻插值法缩放到原来图片的四分之一

> 参考：[cv2.resize()](https://blog.csdn.net/wzhrsh/article/details/101630396)

## 画图 

OpenCV的画图确实要比matplotlib.pyplot()高级一点

> 参考：[matplotlib画直线](https://blog.csdn.net/skyli114/article/details/77508136)

比如画直线等

> 参考：[Python 用 OpenCV 画直线 (3)](https://blog.csdn.net/u011520181/article/details/83999786)
>
> 该博客系列还有画点、圆、矩形、椭圆，写文字、显示图像等内容

`cv2.line(img, pt1, pt2, color[, thickness[, lineType[, shift]]])`

img：要画的圆所在的矩形或图像

pt1：直线起点

pt2：直线终点

color：线条颜色

thickness：线条宽度

lineType：

- 8：8-connected line
- 4：4-connected line
- CV_AA：antialiased line

shift：坐标点小数点位数

`cv2.polylines(img, pts, isClosed, color[, thickness[, lineType[, shift]]])`

- img - 图片
- pts - 多边形顶点（这个该如何写，很有意思，可以用np.array定义变量，但是最后加入到函数中时，还需要再次使用[]将变量括起来）
- isClosed - 是否闭合线段
- color - 颜色

> 参考：[cv2.polylines()绘制多边形](https://drivingc.com/p/5af8f4962392ec4f4727ccce)

`cv2.fillpoly()`

可以填充任意形状的图形

一次可以填充多个图形

`cv2.fillConvexPoly()`

可以用来**填充**凸多边形，只需要提供凸多边形的顶点

> 参考：[cv2.fillConvexPoly()与cv2.fillPoly()填充多边形](https://blog.csdn.net/u012135425/article/details/84983265)

## 视频相关

`cv = cv2.VideoCapture('/dev/video10')`

表示打开摄像头

如果括号里面是视频的文件名（也可以是绝对、相对路径），那么就会调用视频，但是需要注意视频位置需要和python脚本的位置在同一个文件夹中

`ret, frame = cv.read()`

按帧读取视频，有两个返回值:

- ret 是布尔值，如果读取帧正确，会返回true，如果读取到结尾，则返回false

- frame 是每一帧的图像，是一个三维矩阵

`cv2.waitKey()`

等待键盘输入

参数：

- 1：表示延时1ms切换到下一帧
- 0，只显示当前帧，相当于暂停
- 参数过大会因为延时较久而卡顿

`cv2.realse()`

释放摄像头

`cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)`

cvtColor函数用于在图像中不同的色彩空间进行转换，用于后续处理

有RGB、HSV、HSI、LAB、YUV，彩色或者灰度图

灰度图

![灰度图计算](https://pic3.zhimg.com/v2-ddeaaaeab138b73aedecf3e0fad56aba_r.jpg)

位图模式（像素只有0或者1），

`cv2.imshow()`

将图片展示出来

`cv2.erode()`和`cv2.dilate()`

图像的形态学操作，腐蚀和膨胀

- 腐蚀 erode = 变瘦
  - 使用腐蚀可以将图片中的一些毛刺或细小的东西去掉
  - 在原图中的每一个小区域里面取最小值，二值化图像，只要有一个点为0，则都为0
  - 参数
    - 图像
    - kernel
    - iterations，该值越高，图像腐蚀程度越高，只能为整数
- 膨胀 dilate = 变胖
  - 取得局部最大值
  - 参数
    - 图像
    - kernel
    - iterations，该值越高，效果越明显，越胖

![image-20200901161845805](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20200901161845805.png)

`cv2.destroyAllWindows()`

warp：经，经线

`cv2.warpAffine()`

仿射变换函数

可以实现旋转、平移、缩放

变换后的平行线依然是平行的

其变换矩阵至少需要获取三组变换前后对应的点坐标，实现变换

`cv2.warpPerspective(src, M, dsize, dst=None, flags=None, borderMode=None， borderValu=None)`

==透视变换函数==

**可以保持直线不变性，但是平行线可能不再平行**

参数：

- src，输入图像
- dst，输出图像
- **M：2x3的变换矩阵**
- dsize：**变换后输出图像尺寸**
- flag：插值方法,`cv2.INTER_LINEAR`是默认的插值方法，可以调整输入图像的大小；cv2.INTER_AREA 表示收缩，缩小尺寸；如果是放大建议使用cv2.INTER_CUBIC和cv2.INTER_LINEAR
- borderMode：边界像素外扩方式
- borderValue：边界像素插值，默认为0

其**==变换矩阵M==**至少需要**四组**变换前后对应的点坐标

其变换矩阵可以通过`cv2.getPerspectiveTransform()`函数获得

> 参考：[python的cv2.warpAffine()和cv2.warpPerspective()解析对比](https://blog.csdn.net/u012114438/article/details/102613492)
>
> 其变换矩阵也可以通过`cv2.getRotationMatrix2D(center, angle, scale)`函数先获得旋转缩放变换，然后再通过`cv2.warpAffine()`函数对图像进行旋转缩放变换
>
> center：旋转中心坐标
>
> angle：旋转角度
>
> scale：缩放尺度

仿射变换——affine transformation

包括平移translation、旋转rotation、放缩isotropic uniform scaling、剪切、反射reflection

欧式变换：euclidean transformation  刚体变换 rigid transformation，包括平移和旋转

> 参考：[仿射变换及其变换矩阵的理解](https://www.cnblogs.com/shine-lee/p/10950963.html)

## 算法

### 基础

`cv2.split()` 拆分（通道分离）

`cv2.merge()`合并（通道合并）

### 霍夫检测

`cv2.HoughLines()` —— 标准

- dst：输出图像，二值化图像
- **lines：存储着检测到的直线的参数对**，该参数似乎不是必须的
- rho：参数极径 r 以像素值为单位的分辨率，一般是1
- theta：参数极角 \theta 以弧度为单位的分辨率，我们使用 1度（`CV_PI/180`）
- threshold：阈值，一条直线所需的最少的曲线交点
- srn and stn：默认为0

`cv2.HoughLinesp(image, rho, theta, threshold[, lines[, minLineLength[, maxlineGap]]])` —— 基于统计

- image 输入图像，必须是二值图像，推荐使用canny边缘检测的结果图像
- rho累加器的距离分辨率，以像素为单位（参数极径 r）
- theta：累加器的角度分辨率，以弧度标识（参数极角 $\theta$）
- threshold：累加器阈值，int，超过设定阈值才被检测为线段
- **lines：储存着检测到的直线参数对的容器（这里存储的是起始终止点坐标【但是似乎并不会读取，在很多使用中甚至没有出现在参数列表中】）**， 该参数 非必须
- minLineLength：检测线段的最小长度
- maxLineGap：同一方向上，两条线段判断定一条线段的最大允许间隔，超多了设定值，则把两条线段当成一条线段，值越大，允许线段上的断裂越大，越有可能检出潜在的直线

> 参考：[OpenCV-Python 霍夫变换 检测直线，圆形](https://blog.csdn.net/wsp_1138886114/article/details/82936218)
>
> 应用案例：[简易车道线识别（图像与计算机视觉）](https://www.jianshu.com/p/15e3f9d8c121)
>
> ```python
> class HoughLinesP():
>  def __init__(self):
>      # 霍夫
>      self.rho = 1  # distance resolution in pixels of the Hough grid
>      self.theta = np.pi / 180  # angular resolution in radians of the Hough grid
>      self.threshold = 30  # minimum number of votes (intersections in Hough grid cell)
>      self.min_line_length = 30  # minimum number of pixels making up a line
>      self.max_line_gap = 30  # maximum gap in pixels between connectable line segments
>      # line_image = np.copy(image)*0 # creating a blank to draw lines on
> 
>  def houghlinesP(self, img):
>      # return (n,4)
>      try:
>          return cv2.HoughLinesP(img, self.rho, self.theta, self.threshold, np.array([]), self.min_line_length, self.max_line_gap)[:, 0, :]
>      except TypeError:
>          return None
> ```
>
> 从多个使用来看，输出结果似乎是直线的起点和终点，但又好像是直线的斜率和截距

### 滤波

阈值与平滑的实现

`cv2.blur(img, (3, 3))`均值滤波

(3, 3) 是均值滤波的方框的大小

`cv2.boxfilter(img, -1, (3, 3), normalize=True)`方框滤波

normalize=True时，结果与均值滤波结果相同

False，表示对加和后的结果不进行平均操作，大于255的使用255表示

`cv2.Guassiannblur(img, (3, 3), 1)`高斯滤波

1表示$\sigma$，x表示与当前值 的距离，计算出的G(x)表示权重值

`cv2.medianBlur(img, 3)`中值滤波

3表示当前的方框尺寸

该函数用于将9个值进行排序，取中值作为当前值

#### 平滑边缘锯齿 —— 中值滤波

两种滤波：中值滤波、高斯滤波

平滑边缘锯齿

主要步骤：找出轮廓，对轮廓进行处理

`cv2.medianBlur()`

> 参考：[机器学习进阶-阈值与平滑-图像平滑操作(去噪操作) ](https://www.cnblogs.com/my-love-is-python/p/10391923.html)
>
> [3 opencv平滑边缘锯齿代码](https://blog.csdn.net/qq_16949707/article/details/70340090)

## CV_bridge

和ROS相关的OpenCV使用

## 车道线识别

> 参考：
>
> [无人驾驶之车道线检测（一）](https://blog.csdn.net/gavinmiaoc/article/details/88786229)
>
> [无人驾驶技术入门（十四）| 初识图像之初级车道线检测](https://zhuanlan.zhihu.com/p/52623916)

### 关于图片中的车道识别

（1）可以首先进行灰度处理

（2）然后进行边缘提取，突出车道线

- 方法：Canny算法和Sobel算法

（3）感兴趣区域选择（Region of Interest）

- 截取
- 似乎直接选择了一个三角形区域

![image-20200902214326070](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20200902214326070.png)

（4）经典霍夫变换，将左右车道线从复杂的图像中提取出来

> 参考：[经典霍夫变换](https://blog.csdn.net/yuyuntan/article/details/80141392)

图像中使用霍夫变换

- 识别直线；`cv2.HoughLines()`，可以检测不同长度的线段

- 还可以识别圆、椭圆等图形

（5）数据后处理

计算左右车道线的直线方程

- 根据每个线段在图像坐标系下的斜率，判断为左车道线还是优车道线
- 最小二乘拟合

计算左右车道线的上下边界

- 现实世界中左右车道线平行 =》 认为左右车道线**最上和最下的点**对应的y值，就是左右车道线的边界

（6）处理视频

也就是把视频变成一帧帧图像，然后进行处理

> 参考：[无人驾驶技术入门（十五）| 再识图像之高级车道线检测](