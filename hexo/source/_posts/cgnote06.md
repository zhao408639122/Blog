---
title: Computer Graphic笔记(六)——相机、透镜与光场
date: 2020-05-21 17:09:19
categories: 计算机图形学
tags:
---

# Camera: Capture

Shutter 快门，控制光在很微小的时间内进入相机

光线进入相机后传入传感器，传感器记录的是 irradiance

透镜可以聚光。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234518.png" alt="image-20200521230317345" style="zoom:50%;" />

<!-- more -->

针孔摄像机不会有景深，在做光线追踪时使用的就是针孔摄像机。

# Field of view (FOV)

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234529.png" alt="image-20200521230650193" style="zoom:50%;" />

$$
FOV = a \arctan(\frac h {2f})
$$

时长和焦距和传感器大小有关系，一般我们将传感器35mm格式，区分不同的焦距来定义视场

+ $17mm$ 是 $104^\circ$ 广角镜头
+ $50mm$ 是 $47 ^\circ$ 普通镜头
+ $200mm$ 是 $12^\circ$ 微距镜头

在渲染时，胶片和传感器可以是不一样 sensor 和 film

+ 传感器用来记录每个点的irradiance 
+ film指存储的格式等等

## Exposure 曝光

$$
H = T \times E\\
Exposure = time \times irradiance
$$

**Exposure time (T)**

Controlled by shutter

**Irradiance (E)**

Controlled by lens aperture(光圈) and focal length

#### Aperture size

仿照人的瞳孔，用来挡光，使用 f-stop 控制

最大就是和瞳孔一样大。

#### Shutter speed

控制了光进入的时间

#### ISO gain(感光度)

后期处理，当感光原件已经感受到光，值乘ISO值来控制灵敏度

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234540.png" alt="image-20200521232342338" style="zoom: 67%;" />

F数越小，光圈越大

#### ISO(Gain)

ISO值大时，噪声也会相应的放大，调大会导致图糊

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234548.png" alt="image-20200521232756292" style="zoom:67%;" />

#### F-Number(F-stop) : Exposure Levels

FN or F/N，两种写法

并不准确的理解：光圈的直径

光圈越大，进入到相机的光越多。

#### Physical Shutter

当物体运动时可能 Motion Blur

曝光时间越长 Motion Blur越严重

运动模糊可以用来体现运动的速度。

Rolling Shutter问题，高速运动的物体也可能产生物体的扭曲。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234556.png" alt="image-20200521233541947" style="zoom: 50%;" />

#### High-Speed Photography 高速摄影


Normal exposure = extremely  fast  shutter  speed +  (large aperture and/or high ISO)

#### Long -Exposure 延时摄影

长曝光，小光圈

# Thin Lens Approximation

我们研究的时理想化的薄透镜

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234603.png" alt="image-20200521234320600" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234611.png" alt="image-20200521234353595" style="zoom: 33%;" />

$z_i$ 是物距， $z_i$ 是像距
$$
\frac 1 f = \frac 1 {z_i} + \frac 1 {z_o}
$$
<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234618.png" alt="image-20200521234753142" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234639.png" alt="image-20200521234821075" style="zoom:50%;" />

## Defocus Blur

### Circle of confusion（CoC） Size

因为像距过近或者物距过远，远处的一个点就变成一个圆，就变成CoC

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234633.png" alt="image-20200521235239787" style="zoom:67%;" />

$$
\frac C A = \frac{d'} {z_i} = \frac {|z_s - z_i|} {z_i}
$$

所以CoC的大小是和透镜本身的大小或者光圈大小成正比，大光圈更模糊，小光圈不会。

### F-Number (F-stop)

 Formal Definition：The f-number of a lens is defined as the focal length divided by the diameter of the aperture.

Common f-stops on real lenses: 1.4, 2, 2.8, 4.0, 5.6, 8, 11, 16, 22, 32.
$$
C = A \frac {|z_s - z_i|} {z_i} = \frac f N \frac {|z_s - z_i|} {z_i}
$$

## Ray Tracing Ideal Thin Lenses

(One  possible) Setup: 

+ sensor size, focal length and aperture size
+ depth of subject of interest $z_o$ 设置物距
+ Calculate corresponding depth of sensor $z_i$ from thin lens equation.

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234646.png" alt="image-20200522000809668" style="zoom:67%;" />

## Depth of Field 景深

我们认为在成像区域的附近一段区域，CoC都是足够小的，景深就是指该区域所对应的物体平面距离。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234653.png" alt="image-20200522001031851" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200522234700.png" alt="image-20200522001231494" style="zoom:80%;" />

## Light Field / Lumigraph

### The Plenoptic Function 全光函数

模拟人所能看到的所有东西
$$
P(\theta,\phi,\lambda, t)
$$
使用 $\theta, \phi$ 描述角度， $\lambda$ 描述色彩， $t$ 描述时间，就相当于电影

加入三维空间中的坐标，类似全息电影的概念，也可以看作是整个视觉世界。
$$
P(\theta,\phi,\lambda, t, V_x, V_y, V_z)
$$
光场就是从全光函数中提取的一小部分，首先定义几个量

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525151618.png" alt="image-20200523000436005" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525151628.png" alt="image-20200523000457135" style="zoom: 67%;" />

![image-20200525144030720](https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525151636.png)

假设有一个函数记录一个包围盒的任意一个位置向任意一个方向的光的强度，这就是光场，二维的位置，二维的方向。

使用两个平面的上的任意两个点，就可以描述从包围盒射出的任意一个方向的任意一个位置的光线。

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525151645.png" alt="image-20200525144649212" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200525151653.png" alt="image-20200525145749497" style="zoom:67%;" />

 广场照相机存储了整个光场的数据，按照需要取对应的光线就可以。

缺点：需要存储空间大造成分辨率大和成本高的问题。