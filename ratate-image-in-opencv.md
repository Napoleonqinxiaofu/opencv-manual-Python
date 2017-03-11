---
title: 图像旋转 image rotation in opencv
date: 2017-01-08 20:27:04
tags:
---

#### 图像旋转

图像旋转，因为图像其实是离散的像素点构成的，这也就意味着我们可以想象出我们可以将图像围绕任何的点来进行旋转，其实也没有那么强烈的必然关系，毕竟试想一下，我们想让图像旋转，必须要给出两个条件，一是绕那个点或者什么轴旋转，第二个就是旋转多少度。在opencv中并没有直接提供类似于rotate这样的函数，它们提供了一个更为广泛的函数——`getRotationMatrix2D`，看这个名字我们就相应想起来放射变换的那个函数，好了，说远了，现在来看一下我们如何让一个图片旋转我们想要的角度呢？

```bash
#
# 前面加载图片的代码我将会略去，因为我们已经写了不少次了
#

#
# 我们假设我们的图像将会绕着图像的中心点进行旋转，所以下面我们定义一下中心点的坐标
#
centerPoint = (img.shape[1]//2, img.shape[0]//2)

#
# 定一个旋转45度的矩阵，使用getRotationMatrix2D来构造
#
M = cv2.getRotationMatrix2D(centerPoint, 45, 1.0)

#
# 通过warpAffine函数来获取最终的旋转图片
#
rotationImage = cv2.warpAffine(img, M, (img.shape[1], img.shape[0]))

#
# 图像显示的部分也略去
#
```

其实你肯定看得出来，我们上面的代码中我们只有getRotationMatrix2D这个函数的我们没有解释过，现在我们来介绍一下这个函数的参数：

+ centerPoint 我们需要让图像绕着哪一个点进行旋转

+ angle 旋转的角度

+ scale 旋转之后的图片的尺寸大小，一般情况下我们都比较喜欢设置为1.0，因为这样我们的图像分辨率就没有多少的变化

得出相应的旋转矩阵之后，我们就可想平移一样使用放射变换的函数来获取最终的图像了。

#### 定义我们自己的图像旋转函数

我们在前面定义了我们自己的平移函数——translate，现在我们感觉旋转图像需要的代码还是不少的，而且需要记忆的东西不少，所以我们还是自己封装一个旋转的函数吧！如下：

```bash
#
# 旋转图像
#
def rotate( img, centerPoint, angle, scale=1.0 ):
    matrix = cv2.getRotationMatrix2D(centerPoint, angle, scale)
    w = img.shape[1]
    h = img.shape[0]
    rotationImage = cv2.warpAffine(img, matrix, (w, h))

    return rotationImage
```

#### 总结

旋转图像需要两个关键条件，一个是旋转位置，另一个是旋转角度。

下面我们来看一下怎样更改图像的尺寸。