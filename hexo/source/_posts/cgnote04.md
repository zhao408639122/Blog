---
title: Computer Graphic笔记(四)——光线追踪
date: 2020-05-07 22:48:52
tags:
---

# Basic radiometry（辐射度量学）

 路径追踪的基础，仍然基于几何光学来研究，不考虑波动性。

**new term:**  [**Radiant Flux, intensity, irradiance, radiance**](https://www.jianshu.com/p/47f3247b11d5)

### Radiant Energy and Flux (Power)

<!-- more -->

Definition: Radiant Energy is the energy of electromagnetic radiation,  measured in units of joules.
$$
Q\,[\text{J = Joule} ]
$$
Definition: Radiant Flux(Power) is the energy emitted, reflected, transmitted or received, per unit time. (功率)
$$
\Phi \equiv \frac {dQ} {dt} [W =Watt][lm = lumen]^*
$$
lumen：流明，说明了一个光源有多么亮



<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200508001646.png" alt="image-20200507230351845" style="zoom:50%;" />

### Radiant Intensity

Definition: The radiant intensity is the power per unit solid angle （立体角、球面角） emitted by a point  light source.
$$
I(\omega) \equiv \frac {d\Phi} {d\omega} [\frac W {sr}][\frac {lm} {sr} =cd = candela]
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
