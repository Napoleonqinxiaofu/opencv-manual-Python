---
title: equalize histogram
date: 2017-01-12 17:23:35
tags: 平均化像素分布、equalize
---

#### 什么是平均化

我们上一届得到某一张图片的像素值的分布，可以看得出来，一般原始的图像的分布都不会是每一个区间内的所有像素点的数量都相同，所以图像看起来多种多样，那么我们能不能手动地让该图像中的每一个区间的像素点的数目相同呢？这样做当然是可以的，但是这又是为了什么？我的回答是，这样做是为了尝试，有的时候这样做会让我们的图像变得明暗对比很高，更容易对图像进行处理，但是这不是绝对，所以说我说这样做的目的仅仅是尝试。

#### 动手吧

在opencv中可以使用equalizeHist函数来实现这个功能，代码如下：

```bash
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
equalizeImage = cv2.equalizeHist(gray)

#
# 再看一下现在直方数据怎么样
#
print cv2.calcHist([equalizeImage], [0], None,[32], [0, 256])

cv2.imshow("origin image", gray)
cv2.imshow("equalize image", equalizeImage)
cv2.waitKey(0) & 0XFF
cv2.destroyAllWindows()
```

#### 总结

图像像素分布平均化，有的时候能达到将图像的对比度改变的效果，有的时候这种效果对我们很有帮助，但是有的时候却适得其反，所以请不要以为任何操作都会带我们走向成功，还有一定要多尝试。

下一节我们将学习一些新的内容（废话），为什么我要强调新呢？因为我们不再是对图像作变换之类的处理了，我们来处理一些图像的噪声之类的内容，没怎明白？好吧，我就是这个意思，下一节你不就明白了，走起。