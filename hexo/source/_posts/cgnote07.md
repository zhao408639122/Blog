---
title: Computer Graphic笔记(七)——颜色与感知
date: 2020-05-22 23:44:05
categories: 计算机图形学
tags:
---

# Physical Basis of Color

## Spectral Power Distribution(SPD)

谱功率密度

可以描述光在各个波长的分布

**线性性质**： 两个光线同时作用等于SPD叠加。

<!-- more -->

## Color 

颜色是人的感知。

![image-20200525152406664](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155851.png)

视网膜上的棒状细胞能够感知光线的强度，可以得到灰度图

锥形细胞相对少很多，可以感知颜色，分为SML三种细胞，对颜色的感知敏感度各不相同

## Tristimulus Theory of Color

![image-20200525152704313](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155900.png)

视网膜对任何形式的SPD的光线进行感知，将SML值传给大脑，在大脑中显示出图像。

## Metamerism（同色异谱）

各不相同的SPD在积分之后可以展示出相同的颜色。

## Color Reproduction / Matching

因为有同色异谱现象的存在，可以混合颜色来获得给定的颜色。

计算机中采用加色系统，如果将RGB混合会得到白色，而减色系统是将各种颜色不相同的颜料混合会得到黑色。

但是在混合过程中可能会出现负数的情况，因为使用了加色系统无法表示，可以将原颜色减去相应的值来表示

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155908.png" alt="image-20200525153357450" style="zoom: 80%;" />

## CIE RGB Color Matching Experiment

使用三种单色的光，来混合出给定的颜色。

统计出每种波长的光对应的CIE RGB的量，积分之后就可以将对应的SPD表示出来。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155914.png" alt="image-20200525153823363" style="zoom:67%;" />

## Color Spaces

sRGB (standard RGB)

### CIE XYZ 颜色匹配系统

定义好颜色匹配曲线，使用xyz来匹配其他的系统

![image-20200525154403582](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155920.png)

Y代表了亮度。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155926.png" alt="image-20200525154547652" style="zoom:67%;" />

## Gamut

色域就是颜色空间所能表示的颜色范围。

#### HSV Color Space(HSV)

PS中的颜色处理器，可以该表色调，亮度和饱和度

#### CIELAB Space 

LAB也是跟感知有关

![image-20200525154904365](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525155933.png)

使用了互补色。

 