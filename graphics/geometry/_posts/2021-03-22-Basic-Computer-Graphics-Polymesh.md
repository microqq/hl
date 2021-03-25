---
layout: post
title:  "基本3D图形（三）：多边形网格"
date:   2021-03-22
categories: 
    - graphics
    - geometry
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

基本3D图形：多边形网格

- [3. 多边形网格](#3-多边形网格)
  - [3.1 凸多边形与凹多边形](#31-凸多边形与凹多边形)
  - [3.2 连通网格](#32-连通网格)
  - [3.3 流形网格](#33-流形网格)
    - [3.3.1 定义](#331-定义)
    - [3.3.2 可定向流形网格](#332-可定向流形网格)
    - [3.3.3 闭合流形网格](#333-闭合流形网格)
  - [3.4 网格数据结构](#34-网格数据结构)
  - [参考](#参考)

### 3. 多边形网格

多边形网格是顶点、边，以及面的集合，它定义了多面对象形状[<sup>[2]</sup>](#refer-anchor-2)。

多边形网格必须满足以下三个条件[<sup>[1]</sup>](#refer-anchor-1)：

- 每一个顶点必须至少共享一条边；
- 每一条边必须共享一个面；
- 如果多边形网格的面相交，则相交的必须是它的顶点或者面（不允许两个面交叉相交）。

根据边共享的面的数量，通常对边有如下定义：

- 只有一个共享面的边称为 Boundary Edge；
- 有两个或以上共享面的边称为 Interior Edge，Interior Edge 又可以细分为两种：
  - 只有两个共享面的边称为 Manifold Edge；
  - 有三个或者以上共享面的边称为 Junction Edge。

#### 3.1 凸多边形与凹多边形

选择一个多边形的所有边的任意一边向两个方向无限延长为直线，如果多边形的其他所有边都在同一侧，那么称这个多边形为凸多边形；反之，如果多边形其他所有边不都在同一侧，那么称该多边形为凹多边形。

- 凸多边形的性质[<sup>[4]</sup>](#refer-anchor-4)
  - 凸多边形的所有内角都小于 $180^\circ$，且任意凸多边形的外角度和为 $360^\circ$；
  - 凸多边形任意两个顶点连成的线段必然在多边形的内部或边上，同理，凸多边形内部或者边上任意两个顶点连成的线段也必然在多边形内部或边上；
  - 任意顶点角都包含多边形所有的边和其内部及边上所有的顶点；
  - 任何正多边形都是凸多边形；
  - 凸多边形最多只能有三个锐角（否则其外角和大于 $360^\circ$，与第一条性质冲突）；

- 凹多边形的性质[<sup>[6]</sup>](#refer-anchor-6)
  根据凹多边形的定义，其内角至少有一个角度大于 $180^\circ$ 小于 $360^\circ$ 的优角。

#### 3.2 连通网格

对于如下图的多边形网格，利用图论语言表示：

$$
G=<V,E>,\\
V=vertices=\{A,B,C,D,E,F,G...\},\\
E=edges=\{(A,B),(B,C),(C,D),(B,I),(H,L),...\},\\
F=faces=\{(A,B,I),(B,C,I),(A,I,M,L),...\}.
$$

<div align=center>
<img src="/hl/static/img/connected-mesh.png" width=600 />
<p align="center">图 1</p>
</div>

<!-- ![connected-mesh](resources/491538fdec22406a855cc9c5babb8004.png) -->

如果多边形网格中任意两个顶点都有一条边构成的路劲连接，那么就称该多边形网格是连通的。

<div align=center>
<img src="/hl/static/img/connected-mesh-2.png" width=800 />
<p align="center">图 2</p>
</div>

<!-- ![connected-mesh-2](resources/f3812a52d10e45e199495692486d4b16.png) -->

#### 3.3 流形网格

##### 3.3.1 定义

多边形网格是流形网格需要满足三个条件：

- 流形网格首先必须是连通网格；
- 流形网格的每一条边最多只能有两个共享面；
- 入射到一个顶点的多个面和该顶点形成一个闭合或者开放扇形，如图 3。

<div align=center>
<img src="/hl/static/img/closed-opend-fan.png" width=800 />
<p align="center">图 3</p>
</div>

<!-- ![closed-opend-fan](resources/6e506fcd43574fa284f7c062eb346fb5.png) -->

不满足条件的连通的多边形网格称为非流形网格。

##### 3.3.2 可定向流形网格

网格面的方向指的是面的入射顶点的循环次序，如果两个邻接面的共有边的次序是相反的，就认为这两个邻接面方向是相容的，当流形网格的所有邻接面方向相容，那么该流形网格就称为可定向。

<div align=center>
<img src="/hl/static/img/face-orientation.png" width=400 />
<p align="center">图 4</p>
</div>

<!-- ![face-orientation](resources/fd776f7da3754860ae02d937d43aaff8.png) -->

莫比乌斯环和克莱因瓶是不可定向流形网格两个例子。

##### 3.3.3 闭合流形网格

当流形网格的所有顶点都有一个闭合扇形，流形网格没有边界，或者流形网格每一条边都只有两个共享面，就称该流形网格是一个闭合流形网格（排除网格自相交的情况）。

#### 3.4 网格数据结构

- 翼边数据结构
  
- 半边数据结构

#### 参考

<div id="refer-anchor-1"></div>

[1] Philip Schneider, David H. Eberly, Geometric tools for computer graphics

<div id="refer-anchor-2"></div>

[2] [Polygon Mesh-Wiki](https://en.wikipedia.org/wiki/Polygon_mesh)

<div id="refer-anchor-3"></div>

[3] [Introduction to Polygon Meshes-ScratchPixel](https://www.scratchapixel.com/lessons/3d-basic-rendering/introduction-polygon-mesh)

<div id="refer-anchor-4"></div>

[4] [凸多边形-Wiki](https://zh.wikipedia.org/wiki/%E5%87%B8%E5%A4%9A%E8%BE%B9%E5%BD%A2)

<div id="refer-anchor-5"></div>

[5] [内角和外角-Wiki](https://zh.wikipedia.org/wiki/%E5%85%A7%E8%A7%92%E5%92%8C%E5%A4%96%E8%A7%92)

<div id="refer-anchor-6"></div>

[6] [凹多边形-Wiki](https://zh.wikipedia.org/wiki/%E5%87%B9%E5%A4%9A%E8%BE%B9%E5%BD%A2)

<div id="refer-anchor-7"></div>

[7] 周培德，计算几何-算法设计与分析

<div id="refer-anchor-8"></div>

[8] [凸包-Wiki](https://zh.wikipedia.org/wiki/%E5%87%B8%E5%8C%85)

<div id="refer-anchor-9"></div>

[9] Mark de Berg, Otfried Cheong, Marc van Kreveld, Mark Overmars, Computational Geometry Algorithms and Applications

<div id="refer-anchor-10"></div>

[10] Philip Schneider, David H. Eberly, Geometric tools for computer graphics
