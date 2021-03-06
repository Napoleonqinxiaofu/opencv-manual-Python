---
title: 二值化——threshold
date: 2017-01-12 20:58:56
tags: 二值化、threshold
---

#### 二值化图像

顾名思义，二值化图像就是这个图像的像素值就只有两个值，不过这两个值是有一定的讲究的，对于8位无符号整数来说，这个二值化就是最大255和最小0，对于简单二值来说，就是1和0，而不是简单的随便设置一个值，这个需要大注意一下。

那么如何使得一张图像变成一张二值图像呢？简单的方式是我们设定一个阈值，大于这个阈值的像素点我们都设置为255，低于或者等于都设置为0。是不是很简单！不过通常来说，我们通过二值化来获取图像中一些物体的轮廓或者直接提取图像中的物体的，所以我们使用简单的二值化就会使得我们的目标离我们远去，所以我们来看一下还有没有一些更好的办法。

#### 简单的阈值二值化方法

就像上面所说的，我们可以设定一个简单的阈值，小于或者等于这个阈值的像素点将会被设置为0，否则被设置为255，我们来看一下opencv中是如何实现的。

```bash
import cv2
import numpy

imagePath = './images/girl.jpg'

img = cv2.imread( imagePath, 0)

#
# 一般情况下我们都是用灰度图像来进行二值化，而不是使用彩色的
#
T, thresh = cv2.threshold(img, 124, 255, cv2.THRESH_BINARY)

cv2.imshow( "all image", thresh)
cv2.waitKey(0) & 0XFF
cv2.destroyAllWindows()
```

现在来介绍一下threshold的参数以及返回值，首先是它的参数：

+ img 需要二值化的图像

+ thresh 阈值

+ maxValue 大于阈值的像素点会被设置为maxValue

+ type 我们二值化的方式，一般来说有两种方式，一种是大于阈值的像素值被设置为0，另外一种正好反过来，第一种我们可以使用cv2.THRES_BINARY，第二种我们可以使用cv2.THRESH_BINARY_INV，最后的INV表示翻转

现在来介绍一下这个函数的返回值，第一个是我们传递进去的阈值（我不知道返回它干什么），第二个是二值化的图像。

其实挺简单的不是吗？下面我们来看一下一个更好的方法——自适应二值化。

#### 自适应二值化

这个方法还不错吧，这也就意味着我们不用每次都需要手动传递阈值了，让程序自动帮助我们计算比较好的阈值就好了，这个方法的原理是取得一个图片的小窗口，类似于前面讲到的卷积核，然后通过这个小窗口来计算相应的阈值，所以我个人认为自适应阈值相对于简单的阈值方式效果要好一些，来看一下吧！
```bash
thresh = cv2.adaptiveThreshold(img, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 7, 3)
```

这个函数参数还挺多，幸好它只返回二值化图像，所以我们来介绍一下这个函数的参数吧！

+ img 需要二值化的图像

+ maxValue 最大值，我们一般的BGR颜色空间的最大值一般都设置为255，但是别的颜色空间可能就会不一样，所以我们要手动传递这个值

+ adaptiveMethod 这个参数表示我们对一个小窗口的取阈值的方式，像我们的代码就是使用高斯分布的方法来计算，类似于我们前面讲到的高斯模糊，还有其他的平均值阈值之类的（cv2.ADAPTIVE_THRESH_MEAN_C），不过我个人现在觉得他们都差不多，所以就没有细究了。

+ threshholdType 与前面简单阈值函数的type一致，可以取cv2.THRESH_BINARY_INV或者cv2.THRESH_BINARY

+ blockSize 我们定义的小窗口大小（是个奇数）

+ C 是一个常数，这个怎么解释呢？有点儿类似于我们以前解数学题的时候，让我们来设计一个线性函数，那么我们就会假设这个线性函数为`ax + b`，这里的C有点儿类似于b。

#### 总结

二值化的方法在opencv中有两种，一种是简单的阈值方法，一种是自适应的阈值方法，其中我个人对于比较复杂的图像更倾向于使用自适应的方法，这样我们不用自己自己来设定阈值了。这些函数的参数还是比较复杂的希望正在阅读的你还是将其深化以下记忆吧！

下一节我们将学习图像中物理边缘的提取。