---
title: Computer Graphic笔记(三)——几何
date: 2020-04-10 11:27:39
tags:
categories: 计算机图形学
math: true
---

## Represent of geometry

### Implicit

优点：

+ 易于存储，占空间小

+ 容易判断点是否在面上/内

+ 容易做光线求交
+ 对于简单图形没有误差
+ 容易解决流体的变化

缺点：

+ 难以描述复杂物体

#### 表达式表示 

使用表达式直接表示$f(x,y,z)=0$

#### CSG

通过几何的布尔运算表示新几何体

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123324.png" alt="image-20200411114845161" style="zoom: 33%;" />

#### 距离函数SDF

空间的点到想要表示的几何形体上的点的最小距离。

对距离函数做Blending，表示能力很强

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123333.png" alt="image-20200411120031801" style="zoom:33%;" />

#### 水平集

类似距离函数，都是将零的线上的点求出来。

也可以定义到三维上

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123338.png" alt="image-20200411120100470" style="zoom: 50%;" />

#### Fractals分型

类似递归的计算图形

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123345.png" alt="image-20200411120234916" style="zoom: 33%;" />

### Explicit

直接给出或参数映射

容易观测图形 

不易判断点是否在面上/内

**根据需要选择表示方法**

