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

+ 法线surface normal,  n
+ 观测方向 Viewer direction, v
+ 光照方向 l
+ 表面参数(color, shininess) 

只考虑自己的着色，不考虑阴影，局部性

#### 漫反射

点光源随传播距离增大衰减

漫反射的能量
$$
L_d = k_d(I/r^2)max(0, n\cdot l)
$$
与观测方向无关

