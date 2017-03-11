---
title: 绘制图像的直方图——histogram in opencv
date: 2017-01-12 16:07:54
tags: histogram
---

#### 理解图像的直方图

以前我一直以为直方图是一个柱状图，然而那只是我的感觉，你知道人的感觉往往不太对，特别是你没有得到正确答案并且还非常自信的时候。准确地说图像的直方图是一种统计的概念，统计的是图像上所有的像素点的强度分布，举个例子，比如说某一个图像中像素值为20到50的像素点的个数，最终我们将这些统计结果返回来，是不是挺简单的，我隔着屏幕都知道你一定很想自己来实现这儿函数了，我很高兴你这样做，但是先冷静一下。我们能计算出来一张图片的像素值的分布特征，最终我们也想显示出来，所以我们在写自己的直方图的函数的时候我们来看一看别人是怎么写的，譬如opencv里面都提供了哪些参数。

#### opencv中的直方图

在opencv中，可以使用calcHist函数来获取数据，别的没什么好说的，让我们来看一下它的参数有哪些（下面说的可都是必须的参数哦）：

+ images 我们需要获取直方图的图像，需要注意的是我们使用了images而不是image，表示我们可以同时计算多个图像，所以比较合理的坐标是将这些图片都放置进一个列表之中，所以这个参数是一个列表类型的参数

+ channels 通道数，如果是灰度图像的话，那么就是[0]，需要注意的是，这也是一个列表类型的参数。如果是彩色的图像的话，我们可以随意从0到2之间取值，也可以取多个值，表示我们同时计算该图片的多个通道的直方图

+ mask 掩膜，又来了，这个什么意思呢？如果我们有一张掩膜图片，那么opencv就会只计算掩膜图像内为1的像素点的直方信息，如果没有的话，我们需要传递一个None

+ bins 这也是一个列表类型的参数，假如我们前面需要计算有两幅图像，也就是channels的length属性为2，那么我们可以规定将特定像素范围内划分为多少个小范围，那么划分成多少个就需要多少个bins。

+ ranges 这就是我们在上一句中提到的划定像素值的范围的地方，同样，这还是一个列表类型的参数。

现在我们来演示一下这个函数的使用方法，因为opencv没有相关的显示直方图的函数，所以我们需要另外一个Python包——matplotlib，我们需要使用它来绘制我们的直方数据。

```bash
import cv2
import numpy
import matplotlib.pylab as plt

img = cv2.imread('./images/beauty.jpg', 1)

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

hist = cv2.calcHist([gray], [0], None, [40], [0, 256])
print hist

plt.figure(0)
plt.title('histogram')
plt.xlabel("bins")
plt.ylabel("number of each bins")
plt.plot(hist, marker='*', color="red")
plt.show()
```

我们通过cv2.calcHist来获取在某一个像素值区间之内像素的个数，如果你打印出上面的hist的值，你会发现你得到的就是一些统计的结果列表，然后你就可以对这些数据为所欲为了。上面我们只是显示出来了灰度图像的直方图，如果是彩色图像，我们应该怎样来显示呢？很简单，我们只需要每一个通道都计算一次直方数据就好了，不过我们不准备一股脑儿一次就计算完成，因为如果你试一试同时计算两个通道以上的直方数据的时候就会知道opencv提供的API并不是那么好用，先看一下怎样分别显示多个通道的直方图。

```bash
import cv2
import numpy
import matplotlib.pylab as plt

img = cv2.imread('./images/9.jpg', 1)

plt.figure(0)
plt.title('histogram')
plt.xlabel("bins")
plt.ylabel("number of each bins")

for channel, color in zip( cv2.split(img), ('blue', 'green', 'red')):
    hist = cv2.calcHist([channel], [0], None, [45], [0, 256])
    plt.plot(hist, color=color, marker="*")

plt.show()
```

这一节我们的代码放得比较多，一定要搞清楚才往后走哦！

#### 总结

计算图像的直方数据，使用opencv下的calcHist函数来完成，该函数可以同时计算多个图像、多个通道的直方图，不过如果是这样的话，最终的到的结果会比较麻烦。同时，得到相关的数据之后我们可以将这些数据可视化，帮助我们直观理解某一张图像的像素分布。

下一节我们将会学习平均化像素分布的知识。