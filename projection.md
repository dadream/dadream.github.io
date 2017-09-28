# 使用Googl Eearth Engine地图投影介绍:第1部分
> 他山之石，可以攻玉

1. MCD12Q1：土地覆盖数据集
2. sinusoidal projection：数据集原始投影
3. plate carrée equirectangular projection
4. Mercator projection
5. 各种投影方式在美国本土的显示效果

***

```

function(){
	print 'hello world'
}

``` 

地球是圆的，但你的电脑屏幕是平的。地图投影描述了平面上的点，如屏幕上的像素，对应于地球表面上的点。

让我们来探讨一下，这是如何使用一个被称为[MCD12Q1](https://lpdaac.usgs.gov/dataset_discovery/modis/modis_products_table/mcd12q1)的全球土地覆盖数据集，这是用美国宇航局的[MODIS](https://en.wikipedia.org/wiki/Moderate-resolution_imaging_spectroradiometer)卫星仪器的数据制作的许多标准数据产品之一。这个数据集很容易在[Google Earth Engine](http://earthengine.google.com/)中提供，一个用于地理空间数据分析的云平台，如果您喜欢，您可以跟随[GitHub](https://github.com/mdhancher/mdh-examples/blob/master/earth-engine/intro-projections-1.js)上的这个简单的脚本来查看我如何制作本文中的所有图像。

首先让我们在它的本地映射投影中查看这个数据集:

![MCD12Q1土地覆盖数据集为2012年，在原生正弦图投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-INsryu_6JOFCf_Q9XXyO2Q.png?raw=true)

*MCD12Q1土地覆盖数据集为2012年，在原生正弦图投影*

该数据使用[sinusoidal projection](https://en.wikipedia.org/wiki/Sinusoidal_projection)正弦投影，这是一个等面积投影的例子:每个像素对应于地球表面上的一个大小相等的区域。出于这个原因，并由于其简单的数学形式，美国航天局在这一预测中制作并分发了其许多全球数据集(海洋中的方块代表这个特定数据集中的无数据区域)。

Earth Engine公共数据目录中的所有数据都存储在原始地图投影中，在这种情况下是正弦投影。但是，您可以很容易地将数据重新采样到其他投影中进行分析或可视化。让我们来看看该数据在另一个常见的投影的显示方式，[equiectangular projection](https://en.wikipedia.org/wiki/Equirectangular_projection):

![MCD12Q1土地覆盖数据集为2012年，在等角投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-D_CIloak6MfyjqkaV50wTw.png?raw=true)

*equirectangular projection，具有与纬度和经度相对应的坐标*

这种投影，有时也称为plate carrée，是常用的，因为数学特别简单:轴直接对应于纬度和经度。然而，注意到极点附近的区域出现横向伸展。这种投影不保留形状或区域。没有完美的方法来摆平一个球体，所以人们在这些年里创造了许多不同的地图投影，每一个都做出不同的妥协，适合不同的用途。

为了补偿两极附近的这种变形，你可能倾向于垂直拉伸这些区域。如果你这样做，然后你得到另一个著名的地图投影，它可以保留局部形状和角度，虽然它当然不保留面积。这是[墨卡托投影](https://en.wikipedia.org/wiki/Mercator_projection):

<div align=center>
![MCD12Q1土地覆盖数据集为2012年，在墨卡托投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-C_vYU9QOdFhWfbOtvk-iqw.png?raw=true)
</div>

<center>*熟悉的墨卡托投影，常用于网上地图*</center>

这种投影从根本上扭曲了陆地的相对面积，使离赤道更远的地区看起来比实际的大得多。例如，它使格陵兰的规模与非洲的规模相当，实际上它只有澳大利亚的三分之一！墨卡托投影有它的优点，但我们很快就会看到其中之一。

我们一直在研究全球数据，但让我们看看当我们放大美国大陆时会发生什么。首先让我们回到这个土地覆盖数据集的原始正弦投影:

![MCD12Q1土地覆盖数据集为2012年，在其原生正弦图投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-_fjb4wRGFZqs2FvPkYKZIg.png?raw=true)

*美国大陆在这片土地上覆盖着数据的原始正弦投影*

显然，正弦投影是提出这个国家规模数据的一个糟糕的选择。如果我们使用equirectangular投影，情况会有所改善:

![MCD12Q1土地覆盖数据集为2012年，在其原生正弦图投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-9hEOWuZNtXp9_1BGVqQRag.png?raw=true)

*美国大陆在一个equirectangular的投影中*

然而，美国仍然明显地水平拉伸。如果你喜欢这个投影的简单性，如果你聚焦在一个局部区域，那么你可以通过水平缩放整个投影来抵消这种失真。如果你这样做，那么任何局部区域也开始看起来像墨卡托投影:

![MCD12Q1土地覆盖数据集为2012年，在其原生正弦图投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-xe5YauXHnPgZAeRDIqcKjA.png?raw=true)

*美国大陆在墨卡托投影*

您可以认识到美国的这一观点，因为它正是Google地图和其他web地图服务所使用的投影。这有很好的理由。你可以从几个可取的特征得出墨卡托投影:北方总是向上，东方总是向右，如果你放大到任何区域，局部形状和角度将是正确的。墨卡托投影实现了这种局部规模的普遍性，代价是我们上面看到的重大的全球规模扭曲。
美国大陆很大，这些地区的扭曲仍然很重要。特别是，北方国家相对于南方各州似乎太大。因此，美国的地图通常不使用我们迄今讨论过的任何预测。一个常见的选择是阿尔伯斯投影:

![MCD12Q1土地覆盖数据集为2012年，在其原生正弦图投影](https://github.com/dadream/dadream.github.io/blob/master/images/proj-p1/1-v0VUFceCUNzZJaU-yKpz-w.png?raw=true)

*阿尔伯斯等面积圆锥投影，常用于美国大陆的地图集*

这是一个等面积投影的另一个例子，所以它准确地代表所有状态的相对区域。注意，我们西部的北部边界不再是一条直线，即使它向东/西运行。在某种意义上，这实际上是一个更准确的图片:由于地球的曲率，从华盛顿州北部到明尼苏达州北部的最短路径确实通过了加拿大。