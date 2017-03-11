---
title: 图像通道数的分割以及合并——split image channel and merge these channel
date: 2017-01-12 13:30:17
tags: 分割、合并、split、merge
---

#### 彩色图像的分割

我记得《三国演义》的开头就是“天下大势，分久必合，合久必分”，从历史的记载来看，无论中外，确实是这样的，具体什么原因呢咱是一个俗人，就不讨论了，我们只来研究一下图像通道数的分割与合并的方法。

我们前面提到过，伟大的物理学家牛顿先生提出的三原色的概念，所以我们的计算机也基本采用这样的表示方式，对于Python图像的分割通道的操作，我们可以很方便地使用numpy来完成，也可以使用opencv的split函数，下面我们来看一下split是怎样工作的。

```bash
import cv2
import numpy

img = cv2.imread('./images/beauty.jpg', 1)

B, G, R = cv2.split(img)

#
# 或者可以使用numpy数组的切片来获取每一个通道的值
#
"""
B = img[:, :, 0]
G = img[:, :, 1]
R = img[:, :, 2]
"""

cv2.imshow("origin image", img)
cv2.imshow('B', B)
cv2.imshow('G', G)
cv2.imshow('R', R)
cv2.waitKey(0) & 0XFF
cv2.destroyAllWindows()
```

仔细观察一下，你会发现每一个通道的细节部分会有不同，有的会比较黑，而有的比较白，比较黑说明这个值越高，反之则越低。

#### 通道合并

不知道有没有听过张学友的一首歌——《回头太难》，的确，我个人不知道如何使用numpy来将三个通道合并，这就尴尬了，不过好在我们还是可以使用opencv来达到这个目标的，在opencv中提供了merge的函数来完成此事，下面我们将上面得到的三个通道的值打乱一下在进行合并，看看怎么样。

```bash
merged = cv2.merge([R, G, B])

cv2.imshow("merged image", merged)
```

我试了一下，很有意思，突然感觉自己是一个化妆师，专门把人画丑的那种。

#### 总结

有一个段子这样说：假如给你很多钱让你离开你的另一半，你会怎么做？————我会不要脸地分分合合！！！不过我想了想自己，哪有那个机会发财，好伤心。

我们可以将一张彩色图像的三个通道分别获得，并且将其重新合并成一张彩色的图像，其中我们使用到了opencv的split以及merge函数，参数很简单。

下一节我们将会学到绘制某一个图像的直方图。