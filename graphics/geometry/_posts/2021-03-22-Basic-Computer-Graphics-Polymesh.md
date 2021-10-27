---
layout: post
title:  "基本3D图形（三）：多边形网格浅谈"
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

基本3D图形：多边形网格浅谈

- [3. 多边形网格浅谈](#3-多边形网格浅谈)
  - [3.1 凸多边形与凹多边形](#31-凸多边形与凹多边形)
  - [3.2 连通网格](#32-连通网格)
  - [3.3 流形网格](#33-流形网格)
    - [3.3.1 定义](#331-定义)
    - [3.3.2 可定向流形网格](#332-可定向流形网格)
    - [3.3.3 闭合流形网格](#333-闭合流形网格)
  - [3.4 网格数据结构](#34-网格数据结构)
  - [参考](#参考)

### 3. 多边形网格浅谈

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
- 关联到一个顶点的多个面和该顶点形成一个闭合或者开放扇形，如图 3。

<div align=center>
<img src="/hl/static/img/closed-opend-fan.png" width=800 />
<p align="center">图 3</p>
</div>

<!-- ![closed-opend-fan](resources/6e506fcd43574fa284f7c062eb346fb5.png) -->

不满足条件的多边形网格即是非流形网格。

##### 3.3.2 可定向流形网格

网格面的方向指的是面的入射顶点的循环次序，如果两个邻接面的共有边的次序是相反的，就认为这两个邻接面方向是相容的，当流形网格的所有邻接面方向相容，那么该流形网格就称为可定向。比如流形网格的所有面都是顺时针（CW）或者逆时针（CCW）的。

<div align=center>
<img src="/hl/static/img/face-orientation.png" width=400 />
<p align="center">图 4</p>
</div>

<!-- ![face-orientation](resources/fd776f7da3754860ae02d937d43aaff8.png) -->

莫比乌斯环和克莱因瓶是不可定向流形网格两个例子[<sup>[12]</sup>](#refer-anchor-12)，不同的是后者是一个不可定向的闭合曲面。

##### 3.3.3 闭合流形网格

当流形网格的所有顶点都有一个闭合扇形，流形网格没有边界，或者流形网格每一条边都只有两个共享面，就称该流形网格是一个闭合流形网格（排除网格自相交的情况）。

#### 3.4 网格数据结构

- 顶点-顶点结构[<sup>[2]</sup>](#refer-anchor-2)

Vertex-vertex-mesh(VV) 只有一张顶点邻接信息表，这种网格数据结构比较简单，它的特点是需求存储空间比较小，也能够有效的支持形状变化。这种结构缺点也很明显，因为边和面的信息是隐藏的，因此在渲染时需要耗时较多的计算查找边和面的信息，同时不能有效支持动态操作边和面。

<div align=center>
<img src="/hl/static/img/vertex-vertex-mesh.png" width=400 />
<p align="center">图 5(图片来自维基百科<sup>[2]</sup>)</p>
</div>

<!-- ![vertext-vertex-mesh](resources/43091038e958473da3d431fa478ac3b8.png) -->

- 面-顶点结构[<sup>[2]</sup>](#refer-anchor-2)

Face-vertex-mesh 有两张信息表分别是面邻接信息表和顶点邻接信息表，通过面邻接信息表可以迅速通过索引查找关联的顶点数据，通过顶点邻接信息表则可以查找到关联的面数据，这种数据结构对于渲染是很有利的，能够迅速建立索引，同时当顶点位置（或颜色等）发生变化时（非几何形变），只需要重新向 GPU 输送顶点数据即可。缺点是边的信息是隐式的，这种结构不支持直接的边操作，一些动态操作比如分裂或者合并一个面，很难使用这种结构完成。

<div align=center>
<img src="/hl/static/img/face-vertex-mesh.png" width=400 />
<p align="center">图 6(图片来自维基百科<sup>[2]</sup>)</p>
</div>

<!-- ![face-vertex-mesh](resources/6a04e6735d5c4333a3b15b1528e8b558.png) -->

- 翼边数据结构[<sup>[18]</sup>](#refer-anchor-18)

翼边数据结构是基于边的邻接信息存储结构，它有三张邻接信息表，其中最重要的是边邻接信息表，它会存储边的两个端顶点，边的左右两面，以及分别与两个端顶点衔接的四条边，示意图如下：

<div align=center>
<img src="/hl/static/img/winged-edge-data-structure.png" width=400 />
<p align="center">图 7(图片引自<sup>[19]</sup>)</p>
</div>

<!-- ![winged-edge-datastruct](resources/ba84315837cc48018b53a8ae4c835aa2.png) -->
  
中心边邻接关系表可采用双向连接边列表（Doubly-Connected Edge List-DCEL）存储关联的点、面以及邻接边数据，中心边顺时针和者逆时针遍历时确定左侧和右侧前后邻接边。

其他两张邻接表比较简单，如点邻接关系表存储了与该点关联的一条边，同样的面邻接关系表则存储了与该面关联的一条边界边。点与面都可能关联多条边，所以选择不同的关联边，会得到不同的点和面邻接关系表。

结构代码如下：

```c++
struct WingedEdge
{
  WingedVertex *startVertex{nullptr};
  WingedVertex *endVertex{nullptr};

  WingedFace *leftFace{nullptr};
  WingedFace *rightFace{nullptr};

  WingedEdge *leftPreEdge{nullptr}; 
  WingedEdge *rightPreEdge{nullptr}; 
  WingedEdge *leftSucEdge{nullptr}; 
  WingedEdge *rightSucEdge{nullptr}; 
};

struct WingedFace
{
  //face data
  //...

  WingedEdge* *wEdge{nullptr};
};

struct WindgedVertex
{
  //vertex data 
  //...

  WingedEdge* wEdge{nullptr}; 
};
```

由于翼边结构中心边的方向并不明确，在遍历关联面时，这条边可能是顺时针或者逆时针的，所以就需要额外的计算确定遍历时边的方向。

- 半边数据结构[<sup>[15]</sup>](#refer-anchor-15)

半边结构（Half-Edge Data Structure）解决了翼边所遇到的问题，它将一条中心边抽象的拆成两条反向的方向边，每个方向边则属于不同的环，如图：

<div align=center>
<img src="/hl/static/img/half-edge-orientation.png" width=400 />
<p align="center">图 8</p>
</div>

<!-- ![half-edge-orientation](resources/00d0946b0f7a418aa1ad12b4b4d0aed9.png) -->

每一个方向边邻接表只存储半边的起始（或者结束）点、关联面、反向的邻接半边，以及前后两个邻接半边，代码如下：

```c++
struct HalfEdge
{
  HalfVertex* hVertex{nullptr};
  HalfFace* hFace{nullptr};

  HalfEdge* twinHEdge{nullptr};
  HalfEdge* preHEdge{nullptr};
  HalfEdge* sucHEdge{nullptr};
};

struct HalfFace
{
  //face data
  //...

  HalfEdge* hEdge{nullptr};
};

struct HalfVertex
{
  //vertex data
  //...

  HalfEdge* hEdge{nullptr};
};
```

半边结构相对翼边结构而言遍历查询时不需要方向判断，减少了不必要的开销。半边结构不支持非流形网格，如一个边被三个面共享的非流形网格，采用半边结构无法确定边的方向。

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

<div id="refer-anchor-11"></div>

[11] [Manifold-Wiki](https://en.wikipedia.org/wiki/Manifold)

<div id="refer-anchor-12"></div>

[12] [克莱因瓶-Wiki](https://zh.wikipedia.org/wiki/%E5%85%8B%E8%8E%B1%E5%9B%A0%E7%93%B6)

<div id="refer-anchor-13"></div>

[13] [流形-Wiki](https://zh.wikipedia.org/wiki/%E6%B5%81%E5%BD%A2)

<div id="refer-anchor-14"></div>

[14] [Half-Edge Data Structures](https://www.flipcode.com/archives/The_Half-Edge_Data_Structure.shtml)

<div id="refer-anchor-15"></div>

[15] Mario Botsch, Leif Kobbelt, Mark Pauly, Pierre Alliez, Bruno L´evy, Polygon Mesh Processing.

<div id="refer-anchor-16"></div>

[16] James Arvo, Graphics Gemes II

<div id="refer-anchor-17"></div>

[17] [Euler Characteristic-Wiki](https://zh.wikipedia.org/wiki/%E6%AC%A7%E6%8B%89%E7%A4%BA%E6%80%A7%E6%95%B0)

<div id="refer-anchor-18"></div>

[18] [Bruce G. Baumgart,Winged-Edge Polyhedron Representation for Computer Vision](https://web.archive.org/web/20050829135758/http://www.baumgart.org/winged-edge/winged-edge.html)

<div id="refer-anchor-19"></div>

[19] [Winged Edge Data Structure-mtu.edu](https://pages.mtu.edu/~shene/COURSES/cs3621/NOTES/model/winged-e.html)