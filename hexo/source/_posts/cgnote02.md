---
title: Computer Graphic笔记(二)——着色与遮挡
date: 2020-03-27 17:53:30
tags:
categories: 计算机图形学
math: true
---

## 深度测试

### 画家算法

先将远处的画上，用近处的物体去覆盖远处的物体

复杂度为排序 O(n log n)

特殊情况，存在互相遮挡，算法不适用

### Z-Buffer(深度缓存算法)

<!-- more -->

同时生成

+ 带有颜色的帧图
+ 存放深度的z-buffer

利用深度缓存，维护遮挡信息

对每个图元，求每个像素的深度，更新维护的两幅图

```cpp
for (each triangle T)
	for (each sample (x,y,z) in T)
		if (z < zbuffer[x,y]) // closest sample so far
			framebuffer[x,y] = rgb; // update color
			zbuffer[x,y] = z; // update depth
		else
			; // do nothing, this sample is occluded
```

#### 复杂度分析

O(n) 对每个图元

## Shading

### Blinn-phong 反射模型

+ Specular highlights 高光
+ Diffuse reflection 漫反射
+ ambient lighting 反射光

#### 引入

+ 法线surface normal,  n（单位向量）
+ 观测方向 Viewer direction, v（单位向量）
+ 光照方向 l（单位向量）
+ 表面参数(color, shininess) 

只考虑自己的着色，不考虑阴影，局部性

#### 漫反射

点光源随传播距离增大衰减

漫反射的能量
$$
L_d = k_d(I/r^2)max(0, n\cdot l)
$$
与观测方向无关

#### Specular Term

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200328193724.png" alt="image-20200328193722343"  />

当能观测到高光时，半程向量h接近于法线附近
$$
h = \frac{\mathbf v +\mathbf l} {\| \mathbf v + \mathbf l \|}\\

L_s = k_s(I/r^2)max(0, \mathbf n \cdot \mathbf h)^p
$$

#### 环境光

$$
L_a = k_a I _a
$$

环境光是常数在blinn-phong模型中。 

### 着色频率

#### Flat shading

对于每一个三角形，两边叉积求出发现，对于每个三角形着色相同。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200328200126.png" alt="image-20200328195853129" style="zoom: 67%;" />

#### Gouraud Shading

对每个顶点进行着色，在三角形内部每一个像素进行插值。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200328201226.png" alt="image-20200328200402184" style="zoom: 67%;" />

#### Phong shading

对三角形的每个像素插值出法线方向，对于每个像素进行一次着色

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200328214234.png" alt="image-20200328214022256" style="zoom:67%;" />

#### Per-Vertex Normal Vectors

$$
N_v = \frac{\Sigma_iN_i}{\|\Sigma_iN_i\|}
$$

不过在实际应用中，应该对每个三角形的附近边的面的法线对面积进行加权平均。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200328214745.png" alt="image-20200328214743354" style="zoom: 67%;" />

#### Per-Pixel Normal Vectors

插值求得，需要用到重心坐标

## Pipeline

website: ShaderToy

视频资料: Snail Shader Program

## Texture Mapping

使用u，v坐标系映射，三角形每个顶点都对应一个（u，v）值。

三角形内部的（u，v）值，也使用插值求出。

