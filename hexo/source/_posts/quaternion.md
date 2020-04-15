---
title: CG学习笔记——理解四元数（含罗德里格斯公式）
date: 2020-03-19 08:43:51
tags:
categories: 计算机图形学
---

找了一篇[学习资料](https://github.com/Krasjet/quaternion/blob/master/quaternion.pdf)

还有[3blue1brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw)的视频

[Quaternions and 3d rotation, explained interactively](https://www.youtube.com/watch?v=zjMuIxRvygQ)

[Visualizing quaternions (4d numbers) with stereographic projection](https://www.youtube.com/watch?v=d4EgbgTm0Bg)

[Visualizing quaternions----An explorable video series](https://eater.net/quaternions)

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

## 2. 三维空间中的旋转（罗德里格斯公式）

此处关注**轴角式**旋转，欧拉角的表示方法会造成万向锁（Gimbal Lock），而且依赖三个坐标的选定，使用四元数正是为了解决这个问题。轴角式旋转是使旋转更加普遍的情况。

定义四个变量分别为$\theta,x,y,z$，$x,y,z$确定旋转轴，$\theta$确定旋转角度，使用了四个自由度。

### 2.1 旋转的分解

将旋转分解为对沿坐标轴水平与垂直方向的旋转
$$
v'= v'_\parallel + v'_\perp \\
v'_\parallel=v_\parallel=proj_u(v) = \frac{u\cdot v}{u \cdot u} u=(u\cdot v)u\\
v_\perp=v-(u \cdot v)u
$$
对于平行于旋转轴u的操作其实没有被旋转，因为在四维空间中沿旋转轴的旋转被标识为轴的旋转，投影在三维空间中形状不变。

定义垂直与两个相互垂直并且与$u$垂直的向量$v'_\perp,w$，$w$可以使用$u,v'_\perp$求得，并使用两个向量合成垂直方向上的旋转。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200415182614.png" alt="image-20200414234010087" style="zoom: 67%;" />
$$
w=u\times v_\perp\\
\|w\|=\|u\times v_\perp\|\\
\qquad\qquad\qquad\quad =\|u\| \cdot\|v_\perp\|*sin(\pi/2)\\
\!=\|v_\perp\|\\
v'_\perp=v'_v+v'_w=cos(\theta)v_\perp+sin(\theta)w\\=cos(\theta)v_\perp+sin(\theta)(u\times v_\perp)
$$

### 2.2 v的旋转

将以上两个旋转合成可以得到
$$
v'=v'_\|+v'_\perp=v_\|+cos(\theta)v_\perp+sin(\theta)(u\times v_\perp)\\
$$
因为叉乘遵守分配律，
$$
u\times v_\perp=u\times (v - v_\|)\\
\qquad\qquad=u\times v-u\times v_\|\\ \>=u\times v
$$
最后代入得：
$$
v'=cos(\theta)v+(1-cos(\theta))(u\cdot v)u + sin(\theta)(u\times v)
$$
也就是罗德里格斯公式。

## 3. 四元数

$$
q=a+bi+cj+dk\; (a,b,c,d)\in \Reals
$$

其中
$$
i^2=j^2=k^2=ijk=-1
$$
四元数也可以写作向量形式：
$$
q=\begin{bmatrix}a\\b\\c\\d\end{bmatrix}
$$
也可以将实部与虚部分开：
$$
q=[s,\textbf v]\;\;(\textbf v=\begin{bmatrix}x\\y\\z\end{bmatrix},s,x,y,z\in \Reals)
$$

### 3.1 性质

#### 3.1.1 模长（范数）

$$
\|q\|=\sqrt{a^2+b^2+c^2+d^2}\\
\|q\|=\sqrt{s^2+\|\textbf v\|^2}=\sqrt{s^2+\textbf v \cdot \textbf v}
$$

#### 3.1.2 加减法与标量乘法

与复数加减法相同，都满足交换律。

#### 3.1.3 四元数乘法

四元数乘法不满足交换律，通常说左乘或者右乘。
$$
q_1q_2 = (a+bi+cj+dk)(e+ fi+ gj+hk) \\= ae+ afi+ agj+ ahk+ \\ \qquad bei+bfi^2 +bgij+bhik+ \\ \qquad cej+cfji+cgj^2 +chjk+ \\ \quad \enspace  dek+dfki+dgkj+dhk^2
$$
用性质化简（在右手系下）
$$
ijk=-1\\ij=k\\jk=i\\ki=j
$$
<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200415182558.png" alt="image-20200415092100237" style="zoom:50%;" />

于是化简原式得：
$$
q_1q_2= (ae−bf −cg−dh)+\\ \qquad \quad \>(be+ af −dg+ch)i+\\
\qquad \quad \,(ce+df + ag−bh)j+\\ \qquad \;(de−cf +bg+ ah)k
$$
**矩阵形式**：

左乘 $q_1$:
$$
q_1q_2=\begin{bmatrix}a & -b & -c & -d \\ b & a & -d & c\\
c & d & a & -b \\ d & -c & b & a
\end{bmatrix}
\begin{bmatrix} e\\f\\g\\h
\end{bmatrix}
$$
右乘 $q_2$
$$
q_2q_1=\begin{bmatrix}a & -b & -c & -d \\ b & a & d & -c\\c & -d & a & b \\ d & c & -b & a\end{bmatrix}\begin{bmatrix} e\\f\\g\\h\end{bmatrix}
$$

#### 3.1.4 Grassmann积

因为
$$
\textbf v \cdot \textbf u = bf + cg + dh\\
\textbf v \times \textbf u = \begin{vmatrix}\textbf i & \textbf j & \textbf k\\
b & c & d \\ f & g & h\end{vmatrix}\\
=(ch - dg)\textbf i - (bh-df)\textbf j + (bg - cf) \textbf k
$$
将四元数乘法使用点乘和叉乘表示
$$
q_1q_2=[ae - \textbf v \cdot \textbf u , a\textbf u + e\textbf v + \textbf v \times \textbf u]
$$

#### 3.1.5 纯四元数

纯四元数是仅有虚部的四元数，有
$$
vu = [-\textbf v \cdot \textbf u , \textbf v \times \textbf u]
$$

#### 3.1.6 逆和共轭

规定
$$
qq^{-1} = q^{-1}q = 1 \enspace (q \ne 0)
$$
因为四元数与运算是群，左逆元等于右逆元。

定义q的共轭为
$$
q^* = a -bi -cj -dk = [s, -\textbf v]
$$
共轭四元数一个很有用的性质是
$$
qq^*=[s,\textbf v] \cdot [s,-\textbf v]\\
\quad \;=[s^2+\textbf v \cdot \textbf v, 0]\\
=\|q\|^2 \qquad \,
$$
而且与共轭四元数相乘时满足交换律。

而且有：
$$
q^{-1} = \frac {q^*} {\|q\|^2}
$$
很像伴随矩阵的形式，实质上在酉空间（可近似认为是复数域中）矩阵的共轭为伴随矩阵的形式。

当四元数的范数为1时，有：
$$
q^{-1} = \frac {q^*} {1^2} = q^*
$$

### 3.2 四元数与3D旋转

依然将旋转分解，将向量定义为纯四元数。
$$
v = [0,\mathbf v] \qquad \qquad v' = [0,\mathbf {v']} \\
v_\perp = [0,\mathbf{v_\perp}] \qquad\qquad v'_\perp =[0,\mathbf{v'_\perp}]\\ 
v_\| = [0,\mathbf v_\|] \qquad\qquad v'_\| = [0,\mathbf {v'_\|}] \\
u = [0,\mathbf u] \qquad\qquad\qquad\qquad \enspace \;
$$
于是可以得到：
$$
v=v_\|+v_\perp \qquad \qquad v'=v'_\|+v'_\perp
$$

#### 3.2.1 $v_\perp$的旋转

在上文推到的式子有：
$$
\mathbf v'_\perp=cos(\theta)\mathbf v_\perp+sin(\theta)(\mathbf u\times \mathbf v_\perp)
$$
对于叉积，因为我们使用的纯四元数，可以很简单的表示叉积
$$
uv_\perp = [-\mathbf u \cdot \mathbf v_\perp, \mathbf u \times \mathbf v_\perp]
$$
因为$\mathbf v_\perp$正交与$\mathbf u$，所以
$$
uv_\perp = [0, \mathbf u \times \mathbf v_\perp] = \mathbf u \times \mathbf v_\perp
$$
带入原式得
$$
v'_\perp=cos(\theta)v_\perp+sin(\theta)(uv_\perp)\\
\,=(cos(\theta)+sin(\theta)u)v_\perp
$$
注意到，如果将$(cos(\theta)+sin(\theta)u)$看作一个四元数，那我们就可以将旋转写成四元数的乘积，可以得到
$$
v'_\perp = q v_\perp\\
q = [cos(\theta), sin(\theta)\mathbf u]
$$
而且q的模长为1，不会对原变量进行缩放，只会进行旋转。

#### 3.2.2 $v_\|$的旋转

由2.1中式子可得
$$
v'_\|=v_\|
$$

#### 3.2.3 $v$ 的旋转

将上面结果相加可得
$$
v'=v'_\|+v'_\perp = v_\|+qv_\perp 
$$
先证明几个引理再来化简上式。

**Lemma 1**  如果$q=[cos(\theta), sin(\theta)\mathbf u]$，且$\mathbf u$为单位向量，则$q^2= qq = [cos(2\theta),sin(2\theta)\mathbf u]$.

直接使用grassmann积就可以求出。

几何意义是，如果绕着一个轴连续旋转两次相同的角度，那么做出的变换等于直接旋转两倍的同一角度。

用上式与四元数的逆对原式进行变形
$$
v' = v_\| + qv_\perp \qquad \qquad \qquad \qquad\qquad\qquad \qquad \qquad\qquad\qquad\qquad\qquad\enspace\; \\
= pp^{-1}v_\| + ppv_\perp \qquad\qquad (q = p ^ 2 \implies p = [cos(\frac 1 2\theta),sin(\frac 1 2\theta)\mathbf u] \ )
$$
**Lemma 2**  设 $v_\|=[0, \mathbf v_\|]$ 为一个纯四元数，而 $q=[\alpha,\beta\mathbf u]$ ，其中 $\mathbf u $ 为单位向量，$\alpha,\beta \in \Reals$，若 $\mathbf v_\|$ 平行于 $\mathbf u$ ，则$qv_\|=v_\|q$.

**Lemma 3** 设$v_\perp=[0, \mathbf v_\perp]$为一个纯四元数，而$q=[\alpha ,\beta \mathbf u]$，其中$\mathbf u$为单位向量，$\alpha,\beta \in \Reals$，若$v_\perp$正交与$\mathbf u$，则$qv_\perp = v_\perp q^*$.

用上面两个引理来化简原式，并且因为复数模数都为1，所以$q^{-1}=q^*$：
$$
v' = pp^{-1}v_\|+ppv_\perp\\
\quad \;=pv_\|p^{-1} + pv_\perp p^* \\ \;=pvp^{-1} = pvp^*
$$
这样就推出了四元数的旋转公式。

### 3.3 3D旋转的矩阵表示

使用3.1.3中的矩阵表示
$$
qvq^* = L(q)R(q^*) v\\
=\begin{bmatrix}
a^2 +b^2 +c^2 +d^2 & ab−ab−cd+cd & ac+bd−ac−bd & ad−bc+bc−ad \\ab−ab+cd−cd& b^2 + a^2−d^2−c^2& bc−ad−ad+bc& bd+ ac+bd+ac\\ 
ac−bd−ac+bd & bc+ ad+ ad+bc & c^2−d^2 + a^2−b^2& cd+cd−ab−ab\\ ad+bc−bc−ad &bd−ac+bd−ac &cd+cd+ ab+ ab &d^2−c^2−b^2 + a^2
\end{bmatrix}
v\\=\begin{bmatrix}
1 &0& 0& 0\\ 0& 1−2c^2−2d^2& 2bc−2ad &2ac+2bd \\0 &2bc+2ad& 1−2b^2−2d^2& 2cd−2ab\\ 0& 2bd−2ac& 2ab+2cd &1−2b^2−2c^2
\end{bmatrix} \qquad (a^2+b^2+c^2+d^2 = 1)\enspace\,
$$


### 3.4 旋转的复合

同矩阵的对称操作的合并
$$
v''=q_2q_1vq^*_1q^*_2\\ \qquad\enspace =(q_2q_1)v(q_1^*q_2^*)
$$

### 3.5 双倍覆盖

四元数与3D旋转的关系并不是一对一的，同一个3D旋转可以使用两个不同的四元数来表示，对任何单位四元数，$q$与$-q$代表的是同一个旋转，如果$q$代表的是沿着旋转轴$\textbf u$旋转$\theta$度，那么$-q$代表的是沿着相反的旋转轴$-u$旋转$(2\pi - \theta)$度。

所以我们经常说单位四元数与3D旋转有一个**2对1满射同态**关系，或者说单位四元数**双倍覆盖**了3D旋转。

## 4. 四元数插值*