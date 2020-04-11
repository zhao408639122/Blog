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

### Barycentric Coordinate（重心坐标）

已知三角形顶点的信息，插值求出每个像素的信息，使用重心坐标。

三角形内每个元素都可以使用顶点的线性组合表示
$$
(x,y)=\alpha A+\beta B + \gamma C\\
\alpha +\beta + \gamma = 1\; (point \ inside \ triangle)
$$
等式限制了点在三角形所在平面内。

三角形重心为为 $(x,y)=\frac 1 3 A+\frac 1 3 B+\frac 1 3 C$

**投影变换下不能保证重心坐标不变**

## Pipeline

website: ShaderToy

视频资料: Snail Shader Program

## Texture Mapping

使用u，v坐标系映射，三角形每个顶点都对应一个（u，v）值。

三角形内部的（u，v）值，也使用插值求出。

### 想法

使用重心坐标插值出每个像素的（u，v）值，使用纹理映射映射到相应的点，用纹理的$k_d$代替blinn-Phong模型中的漫反射系数。

### 问题一

贴图分辨率不够，会发生问题**Texture Magnification**

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410094949.png" alt="image-20200410094921444" style="zoom: 33%;" />

### 解决方法一：双线性插值Bilinear Interpolation

在水平方向与数值方向分别进行插值，最后将得到的两个点插值

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410095546.png" alt="image-20200410095404484" style="zoom: 40%;" />

### 解决方法二：双三次插值Bincubic Interpolation

### 问题二：纹理分辨率过大

远处摩尔纹，近处锯齿

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410110329.png" alt="image-20200410095958644" style="zoom:30%;" />

可以使用超采样方法，但是计算量过大

### 解决方法：Mipmap

不采样，使用数据结构查询范围的平均值

仅可以做正方形的查询。

把周围像素映射到（u，v）值，近似出一个小正方形，求小正方形会在第几层变成一个像素。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410110339.png" alt="image-20200410104826338" style="zoom: 45%;" />

在非整数层对层与层之间做插值。

对两层都先进行双线性插值，再在层与层之间做一次插值，称为**三线性插值**，整体开销不大。

#### 出现问题：OverBlur

因为mipmap只能查询正方形区域的像素值，图片容易糊掉。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410110347.png" alt="image-20200410105551772" style="zoom:33%;" />

#### 解决办法一：Anisotropic Filtering 各向异性过滤

可以对长条形的区域进行查询，解决了一定问题，但是对于斜着的长条形区域依然无法查询，总开销变为原来的三倍，将原图扩展然后进行Mipmap。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200410110355.png" alt="image-20200410105957097" style="zoom:50%;" />

#### 解决办法二：EWA Filtering

将不规则的形状拆分成不同的圆形，需要多次查询。

## 纹理应用

### 环境光照

环境光照/环境贴图

纹理可以用来表述环境光，环境光可以认为来自无限远处

#### Spherical Environmental Map

将环境光记录在球上并展开，但是会产生扭曲问题，顶部和下方描述不均匀。

#### Cube Map

在球的周围加一个包围盒，从球面延长达到包围盒来计算环境光照。

### 凹凸贴图

可以定义贴图高度，通过贴图高度计算出一个假的法线结果，根据计算出的法线进行着色，不改变几何产生凹凸的效果。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123310.png" alt="image-20200410120057015" style="zoom: 40%;" />

#### Bump Mapping

将每个贴图像素的高度进行扰动，使用扰动的高度和周围的像素的高度差重新计算法线

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200411123300.png" alt="image-20200410121222820" style="zoom:50%;" />

#### 法线的计算

计算扰动曲线的导数，旋转90°

分别计算x与y的导数，然后进行归一化
$$
dp/du=c1*[h(\textbf u+1)-h(\textbf u)] \\
dp/dv=c1*[h(\textbf v+1)-h(\textbf v)] \\
\textbf n = (-dp/du, -dp/dv, 1).normalized()
$$

使用此坐标系重新带入到原坐标系中算出原坐标系中的法线方向。

#### Displacement Mapping 位移贴图

利用贴图真正移动顶点的位置。

要求图元信息足够精细。

### 其他用处

3D噪声贴图

阴影贴图（环境光遮蔽）

三维体渲染