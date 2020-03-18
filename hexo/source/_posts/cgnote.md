---
title: Computer Graphic笔记
date: 2020-03-10 15:45:25
tags:
categories: 计算机图形学
---

> 参加了GAMES101的计算机图形学课程，这里记录课程的笔记合集

## 3D Transformations

### Rotation transformation

![image-20200310142500583](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084811.png)

### Rodrigues‘ Rotation Formula

![image-20200310143409841](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318085300.png)

默认沿着某个轴的方向旋转，默认轴是过原点的，方向是n的方向，角度为$\alpha$。

<!-- more -->

#### *四元数

## Viewing transformation  

### View / Camera Transformation

#### 1. Define the camera 

+ Position $\overrightarrow{e}$
+ Look-at / gaze direction (观测方向) $\hat{g}$
+ Up direction $\hat{t}$

#### 2. Key observation

将相机放在(0,0,0)，向Z轴负方向看，y为向上方向

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084837.png" alt="image-20200310160823745" style="zoom: 80%;" />

#### 3. Transform the camera by $M_{view}$

1. Translates $\overrightarrow{e}$ to origin
2. Rotates $\hat{g}$ to -Z
3. Rotates $\hat{t}$ to Y
4. x自然就对上了
5. ![image-20200310161828112](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084841.png)



### Projection transformation



#### 1. Orthographic projection

+ 平移到原点
+ 缩放到$[-1,1]^3$
![image-20200310165721757](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084848.png)
+   因为向-Z看，近大于远，因此OpenGL使用左手系

#### 2. Perspective projection（透视投影）

![image-20200310172510840](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084851.png)

1. 将frustum“挤”成立方体
2. 做正交投影

![image-20200310171249886](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084854.png)

$$
M_{persp} = M_{ortho}M_{persp\rightarrow ortho}\\
M_{persp\rightarrow ortho}=
\begin{bmatrix}
n & 0 & 0 & 0\\
0 & n & 0 & 0\\
0 & 0 & n + f & -nf\\
0 & 0 & z & 0
\end{bmatrix}
$$
