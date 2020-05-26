---
title: Computer Graphic笔记(五)——材质与外观
date: 2020-05-14 12:59:59
categories: 计算机图形学
tags:
---

# Material and appearance

## Diffuse / Lambertian Material(BRDF)

从Blinn-Phong反射模型入手，漫反射是一束光射入后均匀反射。

所以假设物体完全漫反射（不吸收光），且每个方向射入的 intensity 相同，所以面接受和反射的 irradiance是不变的

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171115.png" alt="image-20200514131836753" style="zoom:50%;" />

$$
L_o (\omega _o)=\int_{H^2} f_rL_i(\omega_i)\cos \theta_i \mathrm d \omega_i \\
\qquad\qquad  = f_rL_i \int_{H^2} \sout {(\omega_i)}\cos \theta_i \mathrm d \omega_i\\
 = \pi f_rL_i \quad \qquad  \enspace \\
 f_r = \frac \rho \pi \qquad
$$


$\rho$ 可以是双通道也可以是RGB

<!-- more -->

## Glossy material (BRDF)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171136.png" alt="image-20200514134058491" style="zoom:50%;" />

接近镜面反射，一般指铜镜等，稍微模糊的抛光面

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171142.png" alt="image-20200514134108771" style="zoom:50%;" />

## Ideal reflective/ refractive material (BSDF*)

**BSDF = BRDF(reflective) + BTDF(transmit)**


 <img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171153.png" alt="image-20200514134335351" style="zoom: 50%;" />

一部分光线被吸收（透明的就不吸收），一部分被折射，一部分镜面反射

### Snell‘s Law (折射定律)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171207.png" alt="image-20200514145054506" style="zoom: 67%;" />

计算折射角的余弦：
$$
\eta _i \sin \theta_i = \eta_t \sin \theta _t \qquad \qquad \qquad \qquad\quad\, \\
\cos \theta _t = \sqrt{1 - \sin^2\theta _t} \qquad  \qquad \qquad\\
= \sqrt{1 - (\frac {\eta_i} {\eta _t})^2 \sin^2\theta _i} \enspace \, \\
 \quad \enspace \,= \sqrt{1 - (\frac {\eta _i} {\eta _t}) ^2 (1 - \cos ^ 2 \theta _i)}
$$
在 $\frac {\eta _i} {\eta_t} > 1$ 的某些情况下，根号下可能无意义，也就不发生折射，也就是全反射现象。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171214.png" alt="image-20200514150208119" style="zoom:67%;" />

###  Fresnel Reflection / Term (菲涅尔项)

解释有多少能量发生了反射，有多少能量发生了折射。 

绝缘体的菲涅尔项：

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171226.png" alt="image-20200514151855931" style="zoom: 67%;" />

 S,P表示的是极化现象，目前的渲染器都不会考虑极化现象。

导体（金属）的菲涅尔项：

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171233.png" alt="image-20200515000003219" style="zoom:67%;" />

Formulae：

![image-20200515000106172](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521180020.png)

## Microfacet Material 微表面材质

 ![image-20200515000613201](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171242.png)

从图中可以看出来，当距离很远时，地球可以近似的看作一个粗糙的平面。

### Microfacet Theory

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171251.png" alt="image-20200515001213500"  />

 假设从远处看到的是粗糙的平面，近处看看到的是几何，微表面可认为是完全的镜子

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171256.png" alt="image-20200515002243392" style="zoom: 80%;" />

可以使用法线分布来描述微表面模型粗糙程度

![image-20200515003109462](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171302.png)

### Isotrpic / Anisotropic materials

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521171309.png" alt="image-20200515003522123"  />

微表面的在方向的分布是否不一致

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521173955.png" alt="image-20200515003538723" style="zoom:67%;" />

**Anisotropic BRDFs**
$$
f_r(\theta_i, \phi_i;\theta_r, \phi_r) \neq f_r(\theta_	i, \theta_r, \phi_r - \phi _i)
$$
入射角与出射角，在旋转时BRDF不改变就是各向同性，否则是各向异性

## Properties of BRDFs

+ Non-negativity

$$
f_r(\omega _i \to \omega _r) \geq 0
$$

+ Linearity

$$
L_r(p, \omega_r) = \int _{H^2} f_r(p, \omega _i \to \omega _r)L_i(p, \omega_i)\cos\theta_i \mathrm d \omega_i
$$

+ Reciprocity Principle 

$$
f_r(\omega _i \to \omega _r) = f_r(\omega _r \to \omega _i)
$$

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174009.png" alt="image-20200515004753649" style="zoom:50%;" />

+ Energy conservation（能量守恒）

$$
\forall \omega _r \int_{H^2}f_r(\omega _i \to \omega _r) \cos\theta_i \mathrm d \omega_i \leq 1
$$

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174028.png" alt="image-20200515005215877" style="zoom: 67%;" />

## Measuring BRDFs

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174039.png" alt="image-20200515005629455" style="zoom:67%;" />

使用各向同性的性质可以加速测量。

# Advanced Light Transport

**Biased vs. Umbiased Monte Carlo Estimators**

如果不管使用多少个样本，期望值等于真实值，则为无偏估计

如果当样本趋向于无穷，结果趋向于正确结果，则为一致估计

## Unbiased light transport methods

### Bidirectional Path Tracing (BDPT)

![image-20200519173759609](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174045.png)

摄像机连出一根光线，光源射出一根光线，然后将线两边的端点连起来，查看光线是否合法。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174051.png" alt="image-20200519173914926" style="zoom:67%;" />

整张图基本都为间接光照，并且摄像头的光线第一次接触物体基本都为漫反射时，BDPT比较有用，但是BDPT比较慢。

### Metropolis Light Transport（MLT）

A Markov Chain Monte Carlo(MCMC) application

马尔科夫链可以在当前样本的周围生成一个新样本来估计值，可以生成一系列以任意的形状为pdf来生成样本。 

当p(x)积分函数形状一致的时候，方差最小，马克可夫链可以生成和积分函数形状近似的样本。

![image-20200519230227169](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174123.png)

在一个path周围生成其他近似的path。

![image-20200519230334528](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174130.png)

caustics 

优点：特别适合做复杂的光路传播。

缺点：难以分析收敛的速度；所有操作都是局部的，有些像素收敛快，有些像素收敛满慢，导致整个画面看起来特别脏

## Biasd light transport methods

### Photon Mapping（光子映射）

![image-20200519231121644](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174142.png)

Very good at handling Specular-Diffuse-Specular (SDS) paths and generating caustics

1. photon tracing

   Emitting photons from the light source, bouncing them around, then recording photons on diffuse surfaces.

   到一个漫反射面就结束

2. photon collection(final gathering)

   Shoot sub-paths from the camera, bouncing them around, until they hit diffuse surfaces.

+ Calculation —— local density estimation

  对任何一个着色点，取最近的n个光子，计算所占的面的面积，计算密度

![image-20200521100054903](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174154.png)

n的取值会影响图的质量

![image-20200521100245426](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174202.png)

**光子映射有偏的原因**

在做密度估计时
$$
\mathrm dN/ \mathrm dA != \Delta N /\Delta A
$$
要想取得更精确的答案，就需要使 $\mathrm dA$ 减小，这就需要更多发射的光子，所以，误差是一直存在的，因为 $\Delta A$ 一定会和 $\mathrm d A$ 有误差，但是是一致方法，极限是趋向正确结果的。

**一个简单的理解bias的方法**

+ Biased == blurry 
+ Consistent == not blurry with infinite #samples

如果选定一个确定的面积来进行密度计算？

因为 $\mathrm d A$ 固定，不会减小，而光子是一定可以增多的，所以是有偏并且不一致的估计。

### Vertex Connection and Merging (VCM)

+ A combination of BDPT and Photon Mapping

![image-20200521105239171](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174216.png)

在BDPT时，如果有两个sub-path很接近，使用Photon Mapping的方法来计算。

## Instant Radiosity (IR)

实施辐射度算法，Many Light 方法

+ 由一个光源打出光到其他地方
+ 将光源看作新的光源 Virtual Point Light (VPL) ，再次打出光

会产生较好的结果，但是在缝隙处容易产生光电，因为在计算 shading points 时，使用平面代替了立体角，两点接近时，容易产生光点，不能做glossy的物体。

![image-20200521110843814](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174452.png)



# Advanced Appearance Modeling

## Non-surface Models

### Participating Media (散射介质)

光线传播过程中可能被吸收或散射，定义函数Phase Function（相位函数）来决定光线在散射介质中如何进行散射。   

在介质中的光线，传播距离由吸收能力决定，传播方向由散射能力决定。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174502.png" alt="image-20200521111947906" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174510.png" alt="image-20200521112227962" style="zoom:33%;" />

散射介质不一定是云雾，有的物体光线进的多，有的进的少。

### Hair Appearance

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174523.png" alt="image-20200521112343580" style="zoom:50%;" />

#### Kajiya-Kay Model

将头发视为圆柱，会反射一个specular的圆锥和一个diffuse的表面。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174531.png" alt="image-20200521112610842" style="zoom: 50%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174605.png" alt="image-20200521112627881" style="zoom:50%;" />

#### Marschner Model

光线打到头发上会反射，同时还有一部分穿透头发，有的穿透两次，有的穿透后在内部反射，再进行穿透。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174615.png" alt="image-20200521112811422" style="zoom:50%;" />

将头发作为一个玻璃的圆柱，并且头发内部有色素，色素会吸收光 Marschner 模型会考虑三种情况

![image-20200521113011976](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174630.png)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174638.png" alt="image-20200521113020478" style="zoom:50%;" />

人的头发不足以描述动物的毛发。

![image-20200521113338810](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174647.png)

相比人，动物的髓质更大，散射程度更大，所以对于动物应该模拟髓质。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174655.png" alt="image-20200521113443169" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174826.png" alt="image-20200521113506473" style="zoom:67%;" />

![image-20200521113628198](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174843.png)

### Granular Material

![image-20200521114312487](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521174834.png)

## Surface Material 

### Translucent Material

Jade、Jellyfish

**Scattering Function（次表面散射）**

可以看作BRDF的延申 BSSRDF
$$
S(x_i, \omega _i, x_o, \omega_o)
$$
从哪个点进入，从哪个方向进入，从哪个点出去，从哪个方向出去
$$
L(x_o, \omega_o)= \int _A \int _{H^2} S(x_i, \omega _i, x_o, \omega_o)L_i(x_i, \omega_i)\cos \theta_i \mathrm d \omega _i \mathrm d A
$$
 将手机打光灯打手指，发现就像底下也有一个光源，所以看作材质上方与低下都有一个光源来进行光线计算。

![image-20200521115419235](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521175052.png)

![image-20200521115445037](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200521175058.png)

### Cloth

布料是一系列缠绕的纤维。经过第一次缠绕会形成不同的股 (Ply)，ply缠绕会形成Yarn或者Fiber.

 将其划分为空间中微小的各自，计算散射与反射的函数，然后将其当作云雾等散射介质来进行计算。

也可以当成实际纤维或者物体表面，根据实际需要都有人用

### Procedural Appearance

定义三维纹理，噪声函数，定义在空间中 $f(x, y, z)$ 可以得到各种不同的纹理，随用随取，不生成，需要用的时候查询。

比如木头，切开也可以获得相应的纹理。