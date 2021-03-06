---
title: opencv 的基本操作——basic image operation in opencv
date: 2017-01-06 21:35:36
tags: 加载图片, 展示图片, 保存图片
---

在这篇教程中，我们将学会如何使用opencv来读取本地文件，并显示在显示板上，最后我们将获得的图片再次保存为另外的图片。

首先我们规划一下我们的工作文件夹，如下：

```bash
./workFolder/

-----------./image/girl.jpg
-----------basicOperation.py
```

我们在我们的工作文件夹（workFolder）下有一个Python文件——basicOperation.py，还有一个image文件夹，里面保存着girl.jpg的图片，这幅图片是赵丽颖的图片。

其次我们在使用opencv的函数之前我们首先来编写一下basicOperation.py的开头部分，如下：

```bash
#_*_coding:utf-8_*_

import cv2

#
# 定义我们的图片路径
#
imagePath = 'image/girl.jpg'

```

#### 读取本地图片

在opencv中我们使用imread函数来获取一个本地图片，并返回这个图片的数据，它的参数如下：

+ imagePath  需要获取的图片的路径

+ imageStyle 获取到的图片的格式（灰度还是彩色的，灰度图片则为0，彩色为1）

现在我们在我们basicOperation.py文件添加如下代码：

```bash
img = cv2.imread(imagePath, 1)
```

我们以彩色的图片个来读取girl.jpg这张图片，并将最终获得图片数据赋值给img变量。

#### 展示图片

上面我们写了如何从读取本地图片，不过谁知道我是不是在胡吹呢，因为我们认为眼见为实，对了，现在我们要通过opencv的`imshow`函数来显示我们刚才获取到的图片，`imshow`函数的参数如下：

+ windowName 这是一个字符串，将会显示在一会儿出现的窗口的上部，表示窗口名称

+ img 需要展示的图片

在basicOperation.py文件添加如下代码：

```bash
cv2.imshow('beautiful girl', img)
cv2.waitKey(0) & 0XFF
cv2.destroyAllWindows()
```

首先我们使用`imshow`函数来显示我们刚才获取到的图片，但是你想，平常我们的电脑窗口不都是有关闭的时候嘛，所以我们又添加了另外的两条语句。`waitKey`表示程序窗口将会等待多长的时间然后将窗口关闭掉，需要等待多长时间就是这个函数传递进去的参数，但是现在我们传递进去的是0，这个表示我们希望程序会等待我们在键盘上按下任何的按键才会将窗口关掉，而后是` & 0XFF`的代码，这是因为我的电脑室是windows64位，所以加上这么一句，并无大碍。

最后我们通过`destroyAllWindows`函数来显式地将窗口关闭掉。鉴于在hexo里面设置图片大小以及位置比较麻烦，我自己就不在这里展示我的图片了，但是我相信你肯定是能看到效果的。

#### 保存图片

显示完了，我们现在想把我们刚才的图片保存下来。你可能会想我是不是弄错了，明明我们是从我们的硬盘里面读取的图片，现在又要将它保存到硬盘里，这不是两一样的图片嘛，干嘛要将其保存起来呢？其实这话倒没有什么错，只是现在我们进行的只是一些简单的操作，当然不会对图片进行一些改动，如果我们后面对图片进行一些改动，比如说我们对图片进行二值化之类的操作，我们最后想将二值化的图像保存下来，我们就需要使用到保存的函数了，不是吗？

说多了，在opencv里面使用到`imwrite`来保存图片，它的参数其实你应该也能想出来，无非就是保存的名称还有需要保存的图片。它的参数如下：

+ filename 

+ image 需要保存的图像

在basicOperation.py文件添加如下代码：

```bash
cv2.imwrite('./images/girl_copy.jpg', img)
```

最终我们在images文件夹下得到了新的图片。


#### 总结

+ 加载图片使用到`imread`

+ 展示图片使用到`imshow`

+ 保存图片使用到`imwrite`

下一篇我们将会学习到有关图像坐标系统以及图像通道数的内容。