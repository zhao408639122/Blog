---
title: Computer Graphic笔记(八)——动画与仿真模拟
date: 2020-05-25 16:00:39
categories: 计算机图形学
tags:
---

# Computer Animation

## Physical Simulation

知道物体上的力，就可以使用物理公式进行模拟。 

##### Mass Spring System 质点模拟系统

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235310.png" alt="image-20200526203228944" style="zoom: 33%;" />

一系列连接着的弹簧和质点。

![image-20200526203458567](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235318.png)

**胡克定律：**
$$
f_{a \to b} = k _s \frac {\mathrm b - \mathrm a} {\| \mathrm b - \mathrm a \|} (\|\mathrm b - \mathrm a \| - l )
$$
引入摩擦力来使系统能量得到损耗。

但是引入摩擦力只能描述外部的力，如果两个质点相对弹簧静止的运动，则也没有能量损耗。

所以引入对b的摩擦 力。

<!-- more -->

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235323.png" alt="image-20200526204403882" style="zoom:67%;" />

 中间对速度与ab之间的位移做点乘，等于对弹簧方向上的速度做投影，可以避免圆周运动等的力。

 **使用质点弹簧系统来模拟布料：**

如果直接使用像纸一样的模拟方法，布料很容易切边，而且不会抵抗将其对折的力 (out-of-plane bending)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235330.png" alt="image-20200526205417140" style="zoom:50%;" />

如果添加斜线，可以抵抗切边，仍不能抵抗非平面的弯折

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235335.png" alt="image-20200526205516527" style="zoom:50%;" />

无论怎样弯，弹簧都会发生长度的变化。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235339.png" alt="image-20200526205619342" style="zoom:67%;" />

*其他方法：FEM 有限元方法，常用于汽车碰撞模拟等

#### Particle Systems

将粒子都建模出来，模拟粒子之间碰撞的力、引力，外部的力，风力等。

粒子系统可以用模拟流体。

## Forward Kinematics 运动学

定义一些简单的关节，从而定义复杂的结构。

通过我们所定义的角度，骨骼长度来算出骨骼尖端的位置。 

### Inverse Kinematics 逆运动学

为了便于艺术家创作，做出界面使得艺术家可以拉着尖端的位置，然后用逆运动学算出骨骼的角度等。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235345.png" alt="image-20200526211811118" style="zoom: 50%;" />

而且有时会产生多种解和无解的情况，一般采取优化的方法。

可以使用梯度下降法求解。

### Rigging

对于一个形状的控制，类似提线木偶的操作，可以看作一个逆运动学的应用。

给一个角色许多不同的控制点， 调整控制点来变化形状等，便于艺术家创作。

两个不同的动作之间可以使用控制点之间的插值方法，类似关键帧的方法，称为**Blend Shapes**。

### Motion Capture 动作捕捉

给真人戴上控制点来捕捉动作

**Pros**

制作简单，更加真实

**Cons**

衣服比较复杂，不一定能捕捉到好的数据，人不能演出动画夸张的效果。

### Facial Motion Capture 面部动作捕捉

在阿凡达中第一次在电影中使用。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235355.png" alt="image-20200526213525697"  />

## Single Particle Simulation

假设有一个理想的速度场，在任何一个位置可以有相对应的速度。

所以对于任何一个位置$x$ ，时间$t$ ，可以得到对应的速度
$$
v(x,t)\\
\frac {dx} {dt} = \dot x = v(x,t)
$$
这是一个常微分方程。

将时间细分成时间小块，对时间进行离散化

### Euler's Method 欧拉方法

$$
x^{t + \Delta t} = x^t + \Delta t \dot x^t \\
\dot x ^ {t + \Delta t} = \dot x ^ t + \Delta t \ddot x ^ t
$$

非常不准，而且容易不稳定。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235404.png" alt="image-20200526220405562" style="zoom: 50%;" />

需要较小的步长来减小误差。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235416.png" alt="image-20200526220538549" style="zoom: 50%;" />

遇到特殊情况不稳定，一切解微分方程的数值方法都会面临的问题：

+ 误差
+ 不稳定

#### Midpoint Method 中点法

每次进行欧拉方法时，取步长的中点，在出发点应用中点的速度，得出计算出的位置，作为一次欧拉布。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235429.png" alt="image-20200526221313707" style="zoom: 50%;" />

$$
x_{mid}= x(t)+\Delta t /2\cdot v(x(t), t) \\
x(t + \Delta t) = x(t) + \Delta t \cdot v(x _{mid}, t)
$$
 实质上优化原理是通过取中点得出了局部的二次项，所以比一次的欧拉方法要更准确。

#### Adaptive Step Size 自适应方法

进行一次欧拉方法后，取中点在进行一次一半时间的欧拉方法，如果两个点距离很远，则应该继续细分。否则使用中点得到的欧拉步。 

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235435.png" alt="image-20200526221842924" style="zoom: 67%;" />

自适应的选用不同的 $\Delta t$ 来进行计算。

#### Implicit Euler Method 隐式/后向欧拉方法

使用下一个时间的导数来更新当前时刻。
$$
x^{t + \Delta t} = x^t + \Delta t \dot x^{t + \Delta t} \\
\dot x ^ {t + \Delta t} = \dot x ^ t + \Delta t \ddot x ^ {t + \Delta t}
$$
不容易求解，一般使用优化或者求根公式来进行。

隐式欧拉方法有较好的稳定性。

#### Determine stability 

+ local truncation error (error step) / total accumulated error (overall)
+ 一般使用阶来决定误差的大小，隐式欧拉方法的误差是一阶的

+ Local truncation error: $O(h^2)$
+ Global truncation error: $O(h)$

$O(h)$的概念：如果有h个点，当h范围减小时，我们期待误差可以以此复杂度的速度减小，阶数越高越好

#### Runge-Kutta Families 龙格库塔方法

RK4是使用最广泛的方法，是四阶的

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235503.png" alt="image-20200526230642772" style="zoom:67%;" />

$h$ 就是 $\Delta t$

### Position-Based / Verlet Integration 

不是一个基于物理的方法

只通过调整不同的位置做出不同的限制。

模拟一个劲度系数无限大的弹簧，当被拉长后会迅速恢复到原来的长度，通过非物理的简化方式直接改变位置。

#### Rigid Body Simulation 刚体模拟

让内部所有的点都按照相同的方式运动。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235511.png" alt="image-20200526231435203" style="zoom: 50%;" />

## Fluid Simulation

### A Simple Position-Based Method

假设水体是由压缩的许多小球组成的，假设水在任何位置都是不可压缩的，密度是一样的。

因为认为水是不可压缩的，当计算出密度有变化，就需要移动小球来使密度不变。

任何一个位置的密度都是关于周围位置小球的函数。求出后使用梯度下降法调整小球。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235522.png" alt="image-20200526232038550" style="zoom:50%;" />

上述方法为拉格朗日方法，俗称质点法。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200526235539.png" alt="image-20200526232138432" style="zoom: 80%;" />

欧拉方法（不同于上述欧拉方法），将空间分为不同的网格，计算每一个格子的值。

结合两种方法 **Material Point Method (MPM)** 物质点法

认为不同的粒子都有不同的材质属性，模拟融化等的过程在格子里做，然后写回粒子中。



