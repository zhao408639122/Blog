---
title: Computer Graphic笔记(一)——变换与光栅化
date: 2020-03-10 15:45:25
tags:
categories: 计算机图形学
math: true
---

> 参加了GAMES101的计算机图形学课程，这里记录课程的笔记合集
>
> 3.27upd. 因为文档太长typora会卡，分块写了

# Transformation

## Homogeneous Coordinates

使用齐次坐标的原因，在进行仿射变换时，不饿能直接时用矩阵来进行变换。

### Affine Transformation

仿射变换，实质上就是平移变换， 也可以理解为对原点的变换。

## 3D Transformations

### Rotation transformation

![image-20200310142500583](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318084811.png)

### Rodrigues Rotation Formula

![image-20200310143409841](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200318085300.png)

默认沿着某个轴的方向旋转，默认轴是过原点的，方向是n的方向，角度为$\alpha$。

***推导** ：放在了四元数的文档中。

<!-- more -->

#### *四元数：已补充

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
0 & 0 & 1 & 0
\end{bmatrix}
$$

$M_{persp\rightarrow ortho}$**的推导**

首先在视锥变换的过程中容易得到
$$
\qquad y' =\frac n z y,\;x'=\frac n z x\\
 \begin{pmatrix}x\\y\\z\\1 \end{pmatrix} \implies 
 \begin{pmatrix}nx/z\\ny/z\\unknown\\1 \end{pmatrix} * z = 
 \begin{pmatrix}nx\\ny\\still\  unknown\\z \end{pmatrix} \\
 \implies M_{persp\rightarrow ortho} = \begin{pmatrix} n & 0 & 0 & 0 \\ 0 & n & 0 & 0 \\ ? & ? & ? & ? \\ 0 & 0 & 1 & 0 \end{pmatrix}\\
$$
因为近平面的上点在进行变换时z不变，用n代替z，容易推断，n与x、y无关
$$
\begin{pmatrix} x\\y\\n\\1  \end{pmatrix} == \begin{pmatrix} nx\\ny\\n^2\\n\end{pmatrix}\implies \begin{pmatrix}0 & 0 & A & B \end{pmatrix}
\begin{pmatrix}x\\y\\n\\1 \end{pmatrix} = n^2 \implies An + B = n^2\\
$$
取原平面的中心，注意在压缩的过程中，原平面中心的z值是不变的所以有
$$
\begin{pmatrix}0 \\ 0 \\ f \\ 1  \end{pmatrix} == \begin{pmatrix} 0 \\ 0 \\f^2 \\ f\end{pmatrix} \implies \begin{pmatrix}0 & 0 & A & B\end{pmatrix}
\begin{pmatrix}0\\0\\f\\1 \end{pmatrix} = f^2\implies Af + B=f^2
$$

 联立可得
$$
\begin{gathered}
An + B = n^2\\
Af + B = f^2
\end{gathered}
\quad \implies
\begin{gathered}
A= n+f\\B=-nf
\end{gathered}
$$
综上可求得
$$
M_{persp\rightarrow ortho}=
\begin{bmatrix}
n & 0 & 0 & 0\\
0 & n & 0 & 0\\
0 & 0 & n + f & -nf\\
0 & 0 & 1 & 0
\end{bmatrix}
$$

#### 实际程序中的计算

程序中一般求投影矩阵需要求四个参数 **eye_fov, aspect_ratio, zNear, zFar** .

+ eye_fov 就是眼睛所能看到的角度
+ aspect_ratio 是宽高比
+ zNear，zFar分别是视锥近平面与原平面的z值

首先由
$$
\tan(\theta/ 2) = H / 2n \\
ar = W/H
$$
W与H是屏幕的宽与高，得到
$$
H = 2n \cdot \tan (\theta / 2) \\
W = 2n\cdot \tan(\theta /2 ) * ar
$$
现在计算投影矩阵
$$
M_{perp} = M_{ortho} M_{persp \to ortho} \qquad \qquad \qquad \qquad\qquad\qquad\qquad\qquad\qquad\\ 
\qquad\qquad\quad\ \ =
\begin{bmatrix}
\frac 2 {r - l} & 0 & 0 & 0 \\
0 & \frac 2 {t - b} & 0 & 0 \\
0 & 0 & \frac 2 {n - f} & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & -\frac {r + l} 2\\
0 & 1 & 0 & -\frac {t + b} 2\\
0 & 0 & 1 & -\frac {n + f} 2\\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
n & 0 & 0 & 0\\
0 & n & 0 & 0 \\
0 & 0 & n + f & -nf \\
0 & 0 & 1 & 0
\end{bmatrix} \\
= 
\begin{bmatrix}
\frac 2 {r - l} & 0 & 0 & -\frac {r + l} {r - l} \\
0 & \frac 2 {t - b} & 0 & -\frac {t + b} {t - b} \\
0 & 0 & \frac 2 {n - f} & -\frac {n + f} {n - f} \\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
n & 0 & 0 & 0\\
0 & n & 0 & 0 \\
0 & 0 & n + f & -nf \\
0 & 0 & 1 & 0
\end{bmatrix} \quad 
\\
=
\begin{bmatrix}
\frac {2n} {r - l} & 0 & 0 & 0 \\
0 & \frac {2n} {t - b} & 0 & 0 \\
0 & 0 & \frac {n + f} {n - f} & \frac {-2nf} {n - f} \\
0 & 0 & 1 & 0
\end{bmatrix} \qquad\qquad\qquad\qquad\qquad\quad\ \
$$


因为
$$
W = r -l\\
H = t - b
$$
将其带入上式可得
$$
M_{perp} = 
\begin{bmatrix}
\frac 1 {ar * \tan(fov / 2)} & 0 & 0 & 0 \\
0 & \frac 1 {\tan(fov  / 2)} & 0  & 0 \\
0 & 0 & \frac {n + f} {n - f} & \frac {-2nf} {n - f} \\
0 & 0 & 1 & 0
\end{bmatrix}
$$

### *Viewport Transform 视口变换

视口变换是将NDC坐标（（-1， 1）的立方体）转换为显示屏幕坐标的过程

 <img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200606203905.png" alt="image-20200606203837990" style="zoom:67%;" />

视口变换中线性映射关系为：
$$
(-1, s_x), (1, s_x + w _ s)\\
(-1, s_y), (1, s_y + h _s) \\
(-1, n _ s), (1, f_s)
$$
所以可以求得
$$
x_s = \frac {w_s} 2(x_n + 1) + s_x\\
y_s = \frac {h_s} 2(y_n + 1) + s_y\\
\qquad \ z_s = \frac {f_s - n_s} 2 z_n + \frac {n_s + f_s} 2
$$
可以由上式做出视口变换的矩阵

### 法线变换

简单距离就可以法线，法线与顶点的变换不相同，变换矩阵相同是，法线与切线向量并不垂直，设切线向量为 T，法线向量为 N，顶点变换矩阵为 M，法线变换矩阵为 G
$$
T' = MT,N' = GN\\
T^TN = 0, \ T'^TN' = 0 \\
\implies (MT)^T(GN) = 0 \\
\qquad \quad \enspace \   T^TM^TGN = 0  \\
\implies M^TG = 1 \qquad \quad   \\
G = (M^T)^-1
$$
所以法线向量的变换矩阵是顶点变换矩阵的逆转置矩阵。

不一定非要求逆矩阵，可以使用求伴随矩阵或者逆操作等方法对计算进行优化。


## Rasterization

### 补充知识：

垂直可视角度

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200320174402.png" alt="image-20200320164844712" style="zoom: 50%;" />

frustum（视锥）的定义：宽高比、垂直/水平可视角度

 pixel 像素

### 1. 采样法

对屏幕空间的每一个点进行采样，计算inside(tri, x, y)。

#### 判断Q是否在三角形内

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200320174414.png" alt="image-20200320172128474" style="zoom: 40%;" />
$$
Evaluate \quad \overrightarrow{P_1Q}\times\overrightarrow{P_1P_2}  \qquad \qquad\quad\\  \overrightarrow{P_2Q}\times\overrightarrow{P_2P_0}\\
\overrightarrow{P_0Q}\times\overrightarrow{P_0P_1}
$$

若三次叉乘符号一致，则Q在三角形内部

####  减少运算——Bounding Box

对于所有点取最小和最大，得到矩形区域，对矩形区域光栅化

Jaggies 锯齿

### 2. Antialiasing

alias走样(jaggies)

 **Idea：Blurring（pre-Filtering) Before Sampling**

#### 频率 

$$
f = \dfrac{1}{T} = \dfrac{\omega}{2\pi}
$$



#### Filtering = Convolution = Averaging 

卷积定理：函数卷积的傅里叶变换是函数傅里叶变换的乘积

采样率越稀疏，傅里叶变换到频谱上越密集

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200327155949.png" alt="image-20200320185704540" style="zoom: 67%;" />

采样不够密集，就会发生走样，用三角形的例子，采样像素少，频谱上就会发生重叠，就会发生走样。

### How to reduce aliasing errror

#### 1. Incresing samping rate

#### 2. Antialiasing

把高通滤波拿掉，然后进行采样

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200327155959.png" alt="image-20200320190025617" style="zoom:67%;" />

### MSAA

是一种反走样的近似解决方法

因为对于每一个像素都要算出来具体的采样面积计算量大，MSAA将每个像素看做多个更小的像素，根据每个像素中采样成功的小像素比例近似计算整个像素中采样面积。第二次采样就包含在了这次采样里。

#### cost

**增加了计算量**

解决方法：

+ 不规则划分
+ 像素复用

### 其他方案

### FXAA

快速近似抗锯齿，图像后期处理，将有锯齿图进行处理，找到边界替换锯齿。

### TAA

核心思想：复用上一帧的信息

#### 超分辨率方法

DLSS

超分辨率问题和抗锯齿不是一种问题，但是本质相同

