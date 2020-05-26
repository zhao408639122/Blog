---
title: Computer Graphic笔记(四)——光线追踪
date: 2020-05-07 22:48:52
categories: 计算机图形学
tags:
---

# Basic radiometry（辐射度量学）

 路径追踪的基础，仍然基于几何光学来研究，不考虑波动性。

**new term:**  [**Radiant Flux, intensity, irradiance, radiance**](https://www.jianshu.com/p/47f3247b11d5)

## term definition

### Radiant Energy and Flux (Power)

<!-- more -->

Definition: Radiant Energy is the e**nergy of electromagnetic radiation**,  measured in units of joules.
$$
Q\,[\text{J = Joule} ]
$$
Definition: Radiant Flux(Power) is the **energy emitted, reflected, transmitted or received, per unit time**. (功率)
$$
\Phi \equiv \frac {\mathrm dQ} {\mathrm dt} [W =Watt][lm = lumen]^*
$$
lumen：流明，说明了一个光源有多么亮



<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200508001646.png" alt="image-20200507230351845" style="zoom:50%;" />

### Radiant Intensity

Definition: The radiant intensity is the **power per unit solid angle** （立体角、球面角） emitted by a point  light source.
$$
I(\omega) \equiv \frac {\mathrm d\Phi} {\mathrm d\omega} [\frac W {sr}][\frac {lm} {sr} =cd = candela]
$$
candela: cd，光源在给定方向的发光强度，基本单位之一。

**solid angle:** ratio of subtended area on sphere to radiu squared 

类比弧度为弧长除以半径，立体角为面积除以半径的平方。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200508001653.png" alt="image-20200507231739294" style="zoom:50%;" />

+ $\Omega = \frac A {r ^ 2}$ 
+ Sphere has $4\pi$ steradians



<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200508001658.png" alt="image-20200508001224640" style="zoom:67%;" />

相当于点光源对单位立体角的亮度，在点光源下：
$$
I = \frac {\Phi} {4\pi}
$$

### Irradiance 

Definition: The irradiance is the **power per unit area** incident on a surface point.
$$
E(x) \equiv \frac {\mathrm d\Phi(x)} {\mathrm dA} \ [\frac W {m^2}][\frac {lm} {m^2} = lux]
$$
面需要和光线垂直。

运用Irradiance来解释Bilin-phone模型。

 <img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005551.png" alt="image-20200512000620004" style="zoom: 80%;" />

### radiance

Definition: The radiance (luminance) is the power emitted, reflected, transmitted or received by a surface, per unit solid angle, per projected unit area. 
$$
L(p, \omega) \equiv \frac {\mathrm  d^2\Phi(p, \omega) } {\mathrm d\omega \mathrm dAcos\theta} \\
 [\frac W {sr\,m^2}][\frac {cd}{m^2} = \frac {lm}{sr \, m^2} = nit]
$$
与Irradiance 的区别： 有无方向

#### Incident Radiance 

从一个立体角射入一个单位面的能量

从Irradiance考虑

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005602.png" alt="image-20200512005519421" style="zoom:80%;" />

#### Exiting Radiance

从一个很小的面射出，朝向某一方向的power

从Intensity考虑

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005610.png" alt="image-20200512005613308" style="zoom: 80%;" />

### Bidrectional Reflectance Distribution Function (BRDF)

描述 单位面积的 Irradiance 如何分配到每个单位角的 radiance\.

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005615.png" alt="image-20200512152111016" style="zoom:67%;" />

 

$$
f_r(\omega _i \to \omega _r)= \frac {\mathrm dL(\omega _r)} {\mathrm dE_i(\omega _i)} =
\frac {\mathrm dL_r(\omega r)} {L_i(\omega _i)\cos \theta _i \mathrm d \omega _i} [\frac 1 {sr}]
$$
描述了物体与光线的作用，定义了物体的材质\.

### The Reflection Equation

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005620.png" alt="image-20200512152259542" style="zoom:67%;" />


$$
L_R(p,\omega_r) = \int_{H^2} f_r (p, \omega _i \to \omega _r) L_i(p, \omega _i) \cos \theta _i \mathrm d \omega_i
$$
 将所有方向来的光做积分,乘BRDF函数,可以求出来到某一个方向的Radiance.

##### challenge

由于反射方程不光需要考虑光源,还需要考虑其他平面反射过来的光,所以问题变成了一个递归的问题.

### The Rendering Equation

物体还有可能发光，所以物体的光表示为：（假定所有方向都朝外）
$$
L_o(p, \omega _ o) = L_e(p, \omega _o) + \int _{\Omega ^ +} L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i) \mathrm d \omega _i
$$
<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005625.png" alt="image-20200512162106058"  />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005631.png" alt="image-20200512162211635" style="zoom:150%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005635.png" alt="image-20200512162219374" style="zoom:150%;" />

![image-20200512164908932](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005640.png)

把所有直接光照和间接光照加起来就是全局光照。

## 蒙特卡洛积分

按照概率密度函数采样，然后求平均。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005645.png" alt="image-20200513200654930" style="zoom:67%;" />

$$
\textrm {Definite  integral} \qquad \int_a ^b = f(x)dx\\
\textrm {Random variable} \qquad X_i \sim p(x) \quad \ \  \\
\textrm {Monte Carlo estimateor} \qquad F_N = \frac 1 N \displaystyle\sum_{i=1}^N \frac{f(X_i)} {p(X_i)}
$$

当随机变量均匀采样时 

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005651.png" alt="image-20200513222641580" style="zoom: 50%;" />

可以推出

$$
\textrm {Definite  integral} \qquad \int_a ^b = f(x)dx \\
\textrm {Random variable} \qquad X_i \sim \frac 1 {b-a} \quad    \\
\quad \textrm {Monte Carlo estimateor} \quad\quad F_N = \frac {b-a} N \displaystyle \sum_{i=1}^N {f(X_i)} \,
$$

**蒙特卡洛积分的推导*：**
$$
E[f(x) ] = \int_{x \in S} f(x)p(x)\mathrm dx \approx \frac 1 N \sum^N _{i=1}f(x^i) \\
define \quad g = fp  \qquad \qquad \qquad \quad \qquad\\
\int_{x\in S} g(x)\mathrm dx \approx \frac 1 N \sum _{i = 1} ^N \frac {g(x^i)} {p(x^i)} \qquad \qquad\\
$$
 蒙特卡洛积分的期望为：
$$
E[\frac 1 N \sum _{i = 1} ^N \frac {g(x^i)} {p(x^i)}] = \frac 1 NE[ \sum _{i = 1} ^N \frac {g(x^i)} {p(x^i)}] \qquad\qquad\qquad \qquad\qquad\  \\
=\frac 1 NN \times \int \frac {g(x)} {p(x)}p (x) \mathrm dx \\
= \int g(x)\mathrm dx \qquad\qquad \quad \enspace
$$


## Path Tracing

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005655.png" alt="image-20200513224746264" style="zoom: 60%;" />

Whitted-Style 只会考虑折射和镜面反射的光线，对于漫反射的光线停止处理，不再进行反射。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005700.png" alt="image-20200513224800317" style="zoom:67%;" />

### 使用渲染方程来求光线追踪

#### A simple Monte Carlo Solution

假定只有直接光照且物体不发光，则
$$
L_o(p, \omega _ o) =\int _{\Omega ^ +} L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i) \mathrm d \omega _i
$$
假设所有光线都是向外的。

使用蒙特卡洛积分
$$
f(x) = L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i) \\
pdf \quad p(\omega_i) = 1/2\pi \ (\text{uniformly  sampling}) \\ \ \\
L_o(p, \omega _ o) =\int _{\Omega ^ +} L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i) \mathrm d \omega _i \\
\qquad\qquad\approx \frac 1 N \displaystyle \sum_{i=1}^N \frac {L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i)} {p(\omega _i)}
$$
<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005705.png" alt="image-20200514000043808" style="zoom:50%;" />

考虑全局光照，如果入射点P其中一个角度打到的是物体Q，则可以看作从Q点的直接光照，所以可以以递归的算法求出全局光照。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005710.png" alt="image-20200514000258541" style="zoom: 50%;" />

注意 -wi是因为光线入射的时候视作向外的。

#### 缺陷

1. 光线数量是指数爆炸级别的。 

 解决方法：

限制光线的数量，当n = 1时，每次一定只选取一根光线，这种方法就是路径追踪。

但是会产生误差。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005721.png" alt="image-20200514000952357" style="zoom: 67%;" />

2. 递归会产生死循环

解决方法：

##### Russian Roulette(RR) 俄罗斯罗盘赌

+ 设置一个概率P
+ 以一定的概率P向某一方向发射一条光线，结果除以P
+ 以1-P的概率不发射光线，则贡献是0

$$
E = P*(L_o/P) + (1 -P) * 0 = L_o
$$

期望还是 $L_o$

3. 效率低

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005728.png" alt="image-20200514002602394" style="zoom:50%;" />

如果光源小的话，在规则采样下，需要相当多的光线才会打到光源。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005735.png" alt="image-20200514002803282" style="zoom: 50%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005740.png" alt="image-20200514003045304" style="zoom:67%;" />

考虑在光源上进行采样，需要1/A的pdf，但是主要蒙特卡洛积分的规则，必须对立体角进行采样，所以考虑如何将光源转化为对立体角 的采样。

所以将dA向单位球做投影

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005744.png" alt="image-20200514003556742" style="zoom:67%;" />

$$
L_o(p, \omega _ o) =\int _{\Omega ^ +} L_i(p, \omega_i)f_r(p, \omega_i, \omega _o) (n \cdot \omega_i) \mathrm d \omega _i \\
\qquad \qquad \quad \ =\int _A L_i(x, \omega  _i) f_r (x, \omega _i, \omega _o) \frac {\cos \theta \cos \theta '} {\|x' -x\|^2} \mathrm d A\\
p(\omega _i) = 1/A
$$

将光线分为两部分，拆分直接光照与间接光照。

![image-20200514004137907](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005748.png)

**处理特殊情况：判断光源与物体是否有阻挡**

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200514005755.png" alt="image-20200514004405089" style="zoom:50%;" />

### 拓展知识

+ 蒙特卡洛使用更优的pdf——重要性采样
+ 更好的随机数序列——Low discrepancy sequence
+ 结合单位球与光源的采样 MIS
+ 对于每个像素打光的平均值 Pixel reconstruction filter
+ Radiance -> color gamma矫正，HDR，颜色空间



