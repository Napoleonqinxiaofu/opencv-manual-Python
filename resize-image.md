---
title: 改变图像的尺寸 resize image
date: 2017-01-11 20:14:17
tags: resize
---

#### 回顾

到此为止，我们已经介绍了两种图像变换的方法了——平移、旋转，现在我们来看一下如何改变图像的尺寸。

想一下，平常我们使用一些看图软件来看遍图像的尺寸真的很容易，只需要操作一些鼠标即可，但是你有想过吗？假如现在我们让图像的尺寸变大，如果只是简单地拉伸，那么我们最终得到的图片会变得很模糊，因为我们知道图像是由很多的像素点组成的，我们将图像的尺寸变大也就是说我们认为地增加了图片中的像素点，所以如果不加处理，那么得到模糊的图片是正常不过的了，具体处理的方法应该不少，有兴趣的话希望自己查一下，我对此并非很精通，所以我在这里不能给你介绍了，抱歉。

#### opencv中的resize函数

改变推向的尺寸大小在opencv里比较方便，不像前两位一样需要至少两行代码，opencv提供了resize这个函数让我们自己操作。我们来看一下这个函数的参数是什么？

###### 必须参数

+ src 我们需要改变尺寸的原图像

+ destination size 这是一个元组，表示我们最终想获得的目标图像的尺寸

###### 可选参数

有的时候人们在使用resize函数的时候都会特别强调它们使用的是什么插值函数，这个插值函数就是我们前面提到过的不只是简单地将图像拉伸的处理函数，专业一点就叫插值函数，resize函数允许我们使用几个插值函数，它的参数就是interpolation，我们可以传递进去opencv里面已经定义好的一些差值方法，譬如：cv2.INTER_AREA，不过既然是可选参数，我们也可以不填写，opencv会自动为我们选择一个插值函数来完成改变尺寸这个事情。关于它的具体介绍可以查阅官方来获得相关的知识，或者如果你感兴趣的话，我在网上找到了一篇博客有很好的介绍——[interpolation参数介绍链接](http://blog.csdn.net/woainishifu/article/details/53260546)


说了这么多，竟然还没有上主菜，不符合咱们的风格，下面我们来上一道代码的主菜：

```bash
#
# 为了保持图像的宽高比，我们提前定义我们的目标图像的尺寸，先定义一下宽高比，乘以1.0是为了防止出现width<height的情况
#
ratio = img.shape[1]*1.0/img.shape[0]

#
# 定义改变尺寸之后的图像的尺寸
#
width = 400
height = width/ratio

resizeImage = cv2.resize(img, (width, int(height)))


#
# 显示图像之类的代码已经忽略
#
```


#### imtool中定义我们的resize函数

我们想自己封装一下我们的resize函数，那么我们就需要提供灵活点的API了，假如说我们想将图片改变为恒高或者恒宽的图片，也就是说我们可以只传递一个宽度或者高度，最终我们得出与原来图片有相同宽高比的图片，如下使我们的resize代码：

```bash
#
# 改变图像的尺寸
#
def resize( img, width=None, height=None, interpolation=cv2.INTER_AREA):
    ratio = None
    size = None

    if width is None and height is None:
        size = (img.shape[1], img.shape[0])
    #当值传递width的时候表示我们想要获得的图片有固定的宽度
    elif width is not None:
        ratio = width / float(img.shape[1])
        height = int(img.shape[0] * ratio)
        size = (width, height)
    elif height is not None:
        ratio = height / float(img.shape[0])
        width = int(img.shape[1] * ratio)
        size = (width, height)

    resizeImage = cv2.resize(img, size, interpolation=interpolation)

    return resizeImage
```

#### 总结

改变图像的尺寸，其实就需要很少的步骤，其中使用的插值方法方法也不是特别难，不过对我个人来说，我只需要知道我该如何使用我们自己的resize函数即可，那你呢？

好了，关于resize函数我们就暂告一个段落了，我们下一节将会学习到如何将图像翻转——Flipping。