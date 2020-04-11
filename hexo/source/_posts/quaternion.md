---
title: CG学习笔记——理解四元数
date: 2020-03-19 08:43:51
tags:
categories: 计算机图形学
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

<!-- more -->

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

### 1.3 复数相乘与2D旋转

复数相乘实质是旋转与缩放的复合？，若有一个复数$z=a+bi$那么与任何一个复数$c$相乘都会将$c$逆时针旋转$\theta=arctan(\frac{b}{a})$，并将其缩放$\lVert z \rVert=\sqrt{a^2+b^2}$倍，若$\|z\|=1$，则可以得到
$$
z = \begin{bmatrix}cos(\theta) & -sin(\theta) \\ sin(\theta) & cos(\theta)\end{bmatrix}
$$
**2D旋转公式（矩阵型）：**
$$
\mathbf v' =\begin{bmatrix}cos(\theta) & -sin(\theta) \\ sin(\theta) & cos(\theta)\end{bmatrix}\mathbf v
$$
而这个矩阵写为复数形式为$cos(\theta) + isin(\theta)$.所以又有

**2D旋转公式（复数积型）：**
$$
\mathbf v' = zv =(cos(\theta ) + isin(\theta)) v
$$

#### 1.3.1 复数的极坐标形

$cos(\theta) + isin(\theta)$可以进行下一步变形，由欧拉公式
$$
cos(\theta) + isin(\theta)=e^{i\theta}
$$
可以将复数变形为
$$
z = \|z\|e^{i\theta}
$$
定义$r=\|z\|$得到复数极坐标形式
$$
z =re^{i\theta}
$$
**2D旋转公式（指数型）**
$$
\mathbf v'=e^{i\theta}\mathbf v
$$

###  1.4 旋转的复合

对向量进行两次旋转
$$
v'' = z_2v'\\ \qquad\,\,=z_2(z_1v)\\ \qquad\,\,=(z_2z_1)v
$$
使用欧拉角形式容易求得
$$
e^{i\theta}\cdot e^{i\phi}=e^{i(\theta+\phi)}
$$
复数相乘等于两次旋转的符合，由于复数满足交换律，复合旋转与先后顺序无关。

## 2. 三维空间中的旋转

此处关注**轴角式**旋转，欧拉角的表示方法会造成万向锁（Gimbal Lock），而且依赖三个坐标的选定，使用四元数正是为了解决这个问题。轴角式旋转是使旋转更加普遍的情况。

