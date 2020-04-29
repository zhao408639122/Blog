---
title: Computer Graphic笔记(三)——几何与光追
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

##### Loop Subdivision 

和循环没有关系 loop是姓氏

1. 将三角形数量增多(取三个中点连接)
2. 调整位置，使得三角形更光滑

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225326.png" alt="image-20200428202941198" style="zoom: 50%;" />

对于新产生的点:

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225342.png" alt="image-20200428203656010" style="zoom:33%;" />

对于老的顶点:

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225356.png" alt="image-20200428203719954" style="zoom:33%;" />

##### Catmull-Clark Subdivision(General Mesh)

用来处理一般的细分,Loop细分只能处理三角形网格的情况.

定义非四边形面和奇异点(度不为4的点)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225406.png" alt="image-20200428204346073" style="zoom:50%;" />

1. 每一条边取中点,面取中心点
2. 调整点的位置

可以发现在进行一次Catmull细分后,所有非四边形面将会变成四边形面,并引入相同数量的奇异点.

更新方式:

将点分为三类:

+ 对于面的中心点,对顶点平均
+ 对于边上的中点,对顶点和面中心点平均
+ 对于老的顶点,对老的点和新产生的点进行平均

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225410.png" alt="image-20200428205231576" style="zoom:50%;" />



#### Mesh simplification 曲面简化

##### 边坍缩Edge collapsing

使用**二次度量误差**，取到一个点使得到其他面距离最小。

每次取出二次度量误差最小的边进行坍缩，更新其他的边，使用堆维护。

#### Mesh Regulariization 网格规范化

# Ray Tracing

## Shadow Mapping（原式阴影生成方法）

1. 从光源渲染光源所能看到的物体，记录深度
2. 从摄像机出发，查询能看到的物体在光源深度图中的深度，如果深度与物体深度不一致，则有物体挡住光线。

### Problems with shadow maps

1. shadow map图分辨率不够造成走样
2. 只能渲染点光源硬阴影，不能渲染软阴影

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225415.png" alt="image-20200429094518730" style="zoom:50%;" />

## Light Rays

1. 光沿直线传播
2. 光并不会发生碰撞（有一些错误）
3. 光经过碰撞到达人的眼睛

### Ray Casting 

1. 从眼睛发出射线，找到最近的像素
2. 与光源连线，查看是否能被照亮，进行着色，写回像素值

## Whitted-Style Ray Tracing

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225420.png" alt="image-20200429212622726" style="zoom: 67%;" />

经过折射与反射的着色都会叠加在像素点上，模拟光线不断弹射的过程。

### Ray-surface Intersection

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225426.png" alt="image-20200429212924226" style="zoom: 80%;" />

定义表示光线的射线
$$
\mathbf r(t) = \mathbf o + t \mathbf d \qquad 0 \leq t \leq \infty
$$

#### 光线与圆求交：

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225429.png" alt="image-20200429213933233" style="zoom:67%;" />

#### 光与隐式表面求交：

$$
\mathbf p:f( \mathbf p) = 0 \\
f(\mathbf o + t \mathbf d) = 0
$$

求正实数解：

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225434.png" alt="image-20200429214213479" style="zoom: 67%;" />

#### 光线与面（三角形）求交：

定义面：

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225437.png" alt="image-20200429215459887" style="zoom:50%;" />

$$
\mathbf p:(\mathbf p - \mathbf p') \cdot \mathbf N = 0\\
(\mathbf p - \mathbf p' ) \cdot \mathbf N = (\mathbf o + t\mathbf d - \mathbf p')
\cdot \mathbf N = 0\\
t = \frac {(\mathbf p' - \mathbf o) \cdot \mathbf N} {\mathbf d \cdot \mathbf N} \qquad \textbf{check:} \ 0 \leq t < \infty
$$

##### MT算法：

因为可以用重心坐标表示三角形所在的平面，所以使用重心坐标列方程，方程可以直接用克拉默法则解：

![image-20200429220317690](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225441.png)

$t,b_1,b_2 $ 都需要为正值。

### 包围盒算法：

用来加速光线与三角形的求交，如果光线碰触不到包围盒，则一定碰触不到三角形。

使用 **AABB包围盒**，可以将长方体看作三个对面的交集。

分别对每个面求出 $t_{min},t_{max}$ ，指进和出平面的时间。 

对三组时间取交集
$$
t_{enter} = max\{t_{min}\},t_{exit} =  m in\{t_{max}\}
$$
<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225448.png" alt="image-20200429224521716" style="zoom:67%;" />

考虑负数情况：

+ $t_{exit} < 0$ ？

  盒子在身后，无交集

+ $t_{exit} >= 0,t_{enter} < 0$ ？

  点在盒子中，有交集。

所以光线与AABB包围盒有交点的条件是当且仅当：
$$
t_{enter} < t_{exit} \&\&  \ t_{exit} >= 0
$$
使用AABB包围盒的原因（Axis-Aligned Bounding Volumes）

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200429225455.png" alt="image-20200429225240546" style="zoom: 67%;" />

简化运算。