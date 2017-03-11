---
title: 掩膜——masking operation
date: 2017-01-12 12:18:01
tags: masking
---

#### 掩膜的个人观点

当我第一次学习到掩膜的时候，我也很费解，这是个什么玩意儿？当时我就很佩服这些翻译人员的努力，取个名字都不能取得通俗易懂一些，要么你多用两个字不行吗？但是答案是不行，因为已经有很多人这么叫了，所以我们还是继续延续它们的叫法吧！

关于这个名词在百度百科上的介绍不是关于计算机视觉的内容，所以我在这里就可以一本正经的瞎说了，我特别喜欢郭德纲的小剧场相声中的瞎说这一节，所以咱也来一个。

举个例子，我的本科毕业的预定的题目是与人脸识别有关的，所以我需要从一张图像中提取人脸的位置来，假设我们现在已经知道人脸的位置（一个矩形），现在我有两种方法来提取这张图像中人脸，一种是直接使用我们前面的图像裁剪获得人脸部分的小图像，另外一种是使用掩膜，将非人脸部分的像素值都设置为背景色（你喜欢设置为多少都无所谓）。掩膜就像大夏天的时候的太阳伞，在太阳伞之下我们就可以不受到阳关的直射，而对于图像，如果我们只想获取某一部分区域的图像，我们可以构造一个掩膜图像，令相对应的区域值为1，其余的为0，然后再使用我们前面讲到的位操作，就可以提取相应的感兴趣区域。

#### 举个例子

说提取人脸就提取人脸，不过我们要使用到一些一些特殊的opencv的函数，这些对于你来说可能会有一些困惑，不过别担心，我们最终只是想获得我们人脸的矩形的坐标，最后构造相应的掩膜图像。

```bash
#_*_coding:utf-8_*_
"""
author:NapoleonQin
email:qxfnapoleon@163.com
weChat:whatever
"""
import cv2
import numpy

cascadePath = "D:\Python\PIL\opencv\cascades\haarcascade_frontalface_default.xml"
faceCascade = cv2.CascadeClassifier(cascadePath)

img = cv2.imread('./images/beauty.jpg', 1)

rects = faceCascade.detectMultiScale( img, 1.1, 5 )

#
# 构建掩膜图像
#
masking = numpy.zeros(img.shape[:2], dtype=numpy.uint8)

#
# 将获得的矩形区域都设置为1
#
for r in rects:
    x, y, w, h = r
    masking[y:y+h, x:x+w] = 1

ROI = cv2.bitwise_and(img, img, mask=masking )

cv2.imshow("face detect", ROI)
cv2.imshow("origin image", img)
cv2.waitKey(0) & 0XFF
cv2.destroyAllWindows()
```

我们使用的是彩色的图片，但是我们的masking掩膜图像只有一个通道，也许你会感到疑惑，或者也许我们的kasking掩膜图像也是三个通道就好理解多了，可以不是这样的。不过你想想，我们想获取某一个部分的图像，难道这一部分区域的坐标在不同的通道上是不一样的吗？不是的，都是一样的，所以这样一来，我们只需要一个通道的掩膜图像就理所当然了。

#### 总结

掩膜的作用就是帮助我们获取我们感兴趣的区域，它将与前面我们学过的位操作相结合得到最终的结果，在上面的代码中，我们只使用了与操作，并且前两个参数我们都是传递了img，也就是说我们最终得到的应该是一张与img有着相同宽和高的全为1的图片，然后由于有掩膜图像的存在，我们的操作只是涉及了掩膜图像中像素值为0的点，其他的点并没有发生改变，就这样，我们的操作已经完成了。

你也可以试一试其他的操作，看看有什么不同。

下一节我们将学习分割通道数以及合并这些通道数的方法。