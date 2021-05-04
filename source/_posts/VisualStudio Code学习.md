---
title: VisualStudio Code学习
date: 2020-07-29 09:15:29
categories:
    - Tools
tags: 
    - software
---

**VScode是强大的代码编辑器**，用一些小技巧重新认识它：

## 1. 开胃菜

调出命令行：**按 ctrl + shift + p**

设置页面：

- 点击左下方设置按钮查询设置内容
- 在设置界面中，会有部分设置内容下方 给出 settings.json 文件

<!--more-->

推荐插件：

| 插件                      | 功能                                                 |
| ------------------------- | :--------------------------------------------------- |
| Bracket Pair Colorizer    | 让每一对括号有自己的颜色；括号越来越多的时候很有用。 |
| vscode-icon               | 不同文件根据代码加图标；增加一点色彩。               |
| Visual Studio IntelliCode | 补全代码；非常有用。                                 |
| remote development        | 远程连接服务器等。                                   |

其他涉及某项语言的插件，可以在用到的时候直接查找即可，甚至微信小程序、VUE、Django、ROS等也有插件可用。

## 2. 协同 Anaconda Prompt

在VSCode中使用Anaconda Prompt的虚拟终端，能够方便地配置好python调试环境等内容

（就可以脱离Anaconda Prompt的束缚了）

打开设置：

### 修改方案一：【勉强实现conda的调用】

主要是修改以下内容：

```javascript
{
    "python.pythonPath": "C:\\ProgramData\\Anaconda3\\python.exe",
    "terminal.integrated.automationShell.windows": "",
    "terminal.integrated.shell.windows": ""
}
```

> 参考：[[在vscode中打开conda的虚拟终端](https://blog.csdn.net/Add_a_cat/article/details/101051759)]

这是一种相对简单的方法，只需要修改配置，就可以进行使用。

**缺点：**目前发现这样的修改后，conda只能进入base环境，而不能进入其他环境，不利于使用

### 修改方案二：【conda完美调用】

首先，修改环境变量，使得能够直接在windows终端（cmd）中直接调用

【注意在conda官方看来，将conda命令加入环境变量是有风险的】

在path（我的电脑-属性-环境变量-系统变量）中，添加anaconda和script的地址，可以打开anaconda prompt输入path直接进行查看

> 参考：[综合处理 'conda' 不是内部或外部命令,也不是可运行的程序 或批处理文件。](https://blog.csdn.net/yelly0/article/details/88426076)

然后，将VSCode的`terminal`地址改为`cmd`，这样就可以像在`cmd`中一样使用`conda`了

【注意：不能直接改成anaconda prompt，原因是不支持，或者说anaconda prompt本身的底层就是cmd】

打开设置，搜索`shell`

主要修改`Terminal>Intergrated>Shell:Windows`

![image-20201105093651583](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20201105093651583.png)

打开`settings.json`

修改`"terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe",`

重新打开vscode，然后再打开终端就可以使用了【如果不重新打开，可能会报一点问题】

注意不要添加`"terminal.integrated.shellArgs.windows"`项，该项也有可能对终端产生一定的影响

## 3. 其他工具

### Git

结合gitlens的使用，比Git terminal更快一步。

前提：认真配置好Git环境，可参考菜鸟教程关于Git的介绍。


### SSH

推荐插件：`remote development`

实现对服务器的远程控制，注意第一次启动时需要对用户名等内容进行一点设置。

*参考：[[使用remote](https://blog.csdn.net/weixin_40373708/article/details/90376258)]*

此外，正常情况下，每次打开新文件夹需要再次输入密码，确实有点麻烦，还可以配置一下ssh-keygen，减少密码输入。

*参考：[[ssh-keygen](https://blog.csdn.net/qq_43143808/article/details/106765470?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)]*

### 工作区

工作区的使用不完全手册

可以把文件夹添加到工作区

选择：

![image-20200812151444206](https://typoraim.oss-cn-shanghai.aliyuncs.com/image/image-20200812151444206.png)

即可在工作区中同时打开两个文件夹了