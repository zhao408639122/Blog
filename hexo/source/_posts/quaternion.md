---
title: CG学习笔记——理解四元数
date: 2020-03-19 08:43:51
tags:
---

找了一篇[学习资料](https://github.com/Krasjet/quaternion/blob/master/quaternion.pdf)，四元数是一种简单的超复数，在我的理解中可以将四元数类比快速傅里叶变换中对复数的是使用——只有复数乘法旋转角度的性质，便于对三维向量的旋转。

## 1.复数

### 1.1 定义

省略对复数定义，我们可以将复数使用向量表示
$$
z = a+bi \\
z =\begin{bmatrix}
a \\ b
\end{bmatrix}
$$

### 1.2 性质

#### 1.2.1 乘法

复数相乘可以表示为矩阵与向量相乘的结果
$$
z_1 =a+bi,z_2=c+di\\
z_1z_2=ac - bd + (bc+ad)i\\
=
\begin{bmatrix}
a & -b\\b & a
\end{bmatrix}
\begin{bmatrix}
c \\d
\end{bmatrix}
$$
可以发现右侧向量为$z_2$，而左侧的$\begin{bmatrix} a & -b\\b & a  \end{bmatrix}$是$z_1$的矩阵形式。尝试把两个向量都使用这种形式，则有
$$
z_1z_2 = \begin{bmatrix} a & -b \\ b & a \end{bmatrix} 
\begin{bmatrix} c & -d \\ d & c\end{bmatrix}\\
=\begin{bmatrix} ac-bd & -(bc+ad) \\ bc + ad & ac - bd\end{bmatrix}=z_2z_1
$$


所以复数相乘是满足交换律的。

#### 1.2.2 模长与共轭

这里大体都记得，注意
$$
\lVert z \rVert = \sqrt{z\bar{z}}
$$

### 1.3复数相乘与2D旋转

复数相乘实质是旋转与缩放的符合，若有一个复数$z=a+bi$那么与任何一个复数$c$相乘都会将$c$逆时针旋转$\theta=arctan(\frac{b}{a})$，并将其缩放$\lVert z \rVert=\sqrt{a^2+b^2}$倍。