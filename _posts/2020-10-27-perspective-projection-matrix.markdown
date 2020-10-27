---
layout: post  
title: "透视投影矩阵"  
date: 2020-09-21  
categories: webgl  
tags: webgl matrix  
---

# 透视投影矩阵  
- 从Frustum内一点投影到近剪裁平面的过程  
- 从近平面到规范化设备坐标系的过程  

$$
  \left[
  \begin{matrix}
    x_c \\
    y_c \\
    z_c \\
    w_c
  \end{matrix}
  \right]
  =
  P*
  \left[
  \begin{matrix}
    x_e \\
    y_e \\
    z_e \\
    w_e
  \end{matrix}
  \right]
$$  
P表示透视投影矩阵 - 将视体内的任意一个点通过P算出要在shader中给glposition中的值(如上)  

$$
  \left[
  \begin{matrix}
    x_n \\
    y_n \\
    z_n
  \end{matrix}
  \right]
  =
  \left[
  \begin{matrix}
    x_c/w_c \\
    y_c/w_c \\
    z_c/w_c 
  \end{matrix}
  \right]
$$

再通过透视除法得到坐标系内的一个坐标(如上)  

**那么如何推导出P**

![原型链]({{ site.url }}/land/pics/ppm-1.jpg)  

- 任意一点投影到近平面  
  相机坐标系中的任意一点p[x<sub>e</sub>,y<sub>e</sub>,z<sub>e</sub>]投影到平面得到点p<pub>'</pub>[x<sub>p</sub>,y<sub>p</sub>,-nearlVal]
  投影的过程是一个相似三角形