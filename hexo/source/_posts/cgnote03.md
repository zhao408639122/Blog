---
title: Computer Graphic笔记(三)——几何
date: 2020-04-10 11:27:39
tags:
categories: 计算机图形学
math: true
---

# Represent of geometry

## Implicit

优点：

+ 易于存储，占空间小

+ 容易判断点是否在面上/内

+ 容易做光线求交
+ 对于简单图形没有误差
+ 容易解决流体的变化

缺点：

+ 难以描述复杂物体

### 表达式表示 

使用表达式直接表示$f(x,y,z)=0$

### CSG

通过几何的布尔运算表示新几何体

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123324.png" alt="image-20200411114845161" style="zoom: 33%;" />

<!-- more -->

### 距离函数SDF

空间的点到想要表示的几何形体上的点的最小距离。

对距离函数做Blending，表示能力很强

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123333.png" alt="image-20200411120031801" style="zoom:33%;" />

### 水平集

类似距离函数，都是将零的线上的点求出来。

也可以定义到三维上

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123338.png" alt="image-20200411120100470" style="zoom: 50%;" />

### Fractals分型

类似递归的计算图形

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123345.png" alt="image-20200411120234916" style="zoom: 33%;" />

## Explicit

直接给出或参数映射

容易观测图形 

不易判断点是否在面上/内

**根据需要选择表示方法**

### Point Cloud

可以表示任何面，扫描出来的是点云

可以表示多边形的面

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200417122153.png" alt="image-20200417094358292" style="zoom: 33%;" />

.obj文件存储模型信息，顶点法线纹理，以及他们的关系

# 曲线和曲面

## Curves

### 贝塞尔曲线-显示表示方法

定义起始点与终点及起始方向和终止方向，得到光滑曲线。

#### de Casteljau Algorithm

每次对所有线段进行插值，然后连接两条相邻线段的插值点，直到最后变为一条线段，插值找出最后的点

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200417122203.png" alt="image-20200417095735686" style="zoom: 50%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200417122210.png" alt="image-20200417095746068" style="zoom:33%;" />

#### 贝塞尔曲线代数公式

给定n+1个控制点——伯恩斯坦多项式

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200417122245.png" alt="image-20200417102045899" style="zoom: 33%;" />
$$
B_i^n(t)=\begin{pmatrix}n \\i \end{pmatrix}t^i(1-t)^{n-i}
$$
所以贝塞尔曲线是对控制点的二次项多项式系数的加权

**优点**：做仿射变换的时候可以直接对点再计算一边

**凸包性质**：贝塞尔曲线在控制点的凸包内

#### Piecewise 逐段贝塞尔曲线

点多的时候不易控制，每次用相邻的点定义贝塞尔曲线

一般选四个控制点三次贝塞尔曲线分片，Piecewise Cubic Bezier 

Photoshop钢笔工具

**C0连续**：第一段的终止点等于第二段的起点

**C1连续**：$a_n =b_0=\frac 1 2 (a_{n-1}+b_1)$，曲线在交点处表现为光滑。

#### Splines 样条曲线

在任何的地方都会满足连续性。

##### Basis Splines

基函数样条

**局部性**：改变一个点，仅改变一个局部的曲线

## Surface 曲面

### 贝塞尔曲面

在两个方向上分别求贝塞尔曲线，使用两个系数$t_1,t_2$，或者$(u, v)$，求两个方向上的贝塞尔曲面。

### 网格 Mesh

#### Mesh subdivision 曲面细分

#### Mesh simplification 曲面简化

#### Mesh Regulariization 网格规范化