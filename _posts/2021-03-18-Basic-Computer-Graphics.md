---
layout: post
title:  "基本3D图形"
date:   2021-03-18
categories: graphics
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

基本3D图形

- [1. 直线](#1-直线)
  - [1.1 2D 直线](#11-2d-直线)
  - [1.2 3D 直线](#12-3d-直线)
  - [1.3 直线光栅算法](#13-直线光栅算法)
  - [1.4 三角形光栅化](#14-三角形光栅化)
  - [参考](#参考)
- [2. 平面](#2-平面)
- [3. 多边形网格](#3-多边形网格)
- [4. 二次曲面](#4-二次曲面)
- [5. 圆环面（体）](#5-圆环面体)
- [6. 多项式曲线](#6-多项式曲线)
- [7. 多项式曲面](#7-多项式曲面)


### 1. 直线

在我们直观的意识里，直线就是一条没有弯曲，无限延长的线。这里我们只讨论欧几里得几何，在欧几里得几何体系中有一条关于直线的公理：过两点有且只有一条直线，这就意味着两点确定一条直线[<sup>[1]</sup>](#refer-anchor-1)。下面我们讨论直线方程在直角坐标系（笛卡尔坐标系）的表达形式。

#### 1.1 2D 直线

* **一般式**[<sup>[1]</sup>](#refer-anchor-1)

$$Ax + By + C = 0.$$

上式不是直线唯一表达形式，当满足约束条件 $A\geq0$ 且 $GCD(A,B,C)=1$（最大公约数）时，直线有唯一表达形式。

* **斜截式**

顾名思义，斜是斜率（slope），截便是截距的意思：
$$y = kx  + b.$$

* **截距式**

假设直线 $x$ 轴的截距等于 $a$，$y$ 轴截距等于 $b$，那么：
$$\frac{x}{a} + \frac{y}{b} = 1.$$

* **两点式**

假设直线穿过两点 $(x_1,y_1)$，$(x_2,y_2)$，则有方程组:

$$
\begin{cases}
y = kx + b,\\
y_1 = kx_1 + b,\\
y_2 = kx_2 + b,
\end{cases}\Rightarrow
$$

$$\frac{x - x_1}{x_2 - x_1} = \frac{y- y_1}{y_2 - y_1}.$$

已知直线方程的一般式 $Ax + By + C = 0$，直线穿过两点 $(x_1,y_1)$，$(x_2,y_2)$，可联立线性方程组得：

$$
\begin{cases}
Ax + By + C = 0,\\
Ax_1 + By_1 + C = 0,\\
Ax_2 + By_2 + C = 0.
\end{cases}
$$

可以将方程组看成是关于 $(A,B,C)$ 的三元一次齐次线性方程组，当其行列式等于零的时候，方程组有解且有无数解，否则，只有零解。所以直线也可以用行列式表示[<sup>[1]</sup>](#refer-anchor-1)：

$$
Det =
\begin{vmatrix} x&y&1 \\
x_1&y_1&1 \\
x_2&y_2&1
\end{vmatrix}
= 0.
$$

* **点斜式**

从名称上看，点斜式就是已知直线上一点 $(x_0, y_0)$，和斜率 $k$：
$$
y - y_0 = k(x - x_0).
$$

* **向量式**

直线本身是没有方向的，但是直线可以看成是一条射线的延展，已知点 $p_0 = (x_0, y_0)$，且射线方向为 $\vec{v}$，求直线上任一点 $p$：
$$
p = p_0 + \vec{v}t\quad(t\in{\mathbb{R}})
$$

* **参数式**

参数式可由向量式展开得到，假设$\vec{v}=(v_x, v_y)$，则：
$$
\begin{cases}
x = x_0 + v_xt,\\
y = y_0 + v_yt.
\end{cases}
$$

#### 1.2 3D 直线

* **一般式**[<sup>[1]</sup>](#refer-anchor-1)

在三维空间中，两个不平行的平面相交于直线 $l$，因此，联立两个平面方程：
$$
\begin{cases}
A_1x + B_1y + C_1z + D1 = 0,\\
A_2x + B_2y + C_2z + D2 = 0.
\end{cases}
\Leftrightarrow
\begin{cases}
A_1x + B_1y + C_1z = -D_1,\\
A_2x + B_2y + C_2z = -D_2.
\end{cases}
$$

从上面的方程组可以看出，方程组有三个未知数 $(x, y, z)$，但只有两个方程，这意味着方程组有无数个解，亦即直线 $l$ 上无限多个点。上面直线方程组表达式不是唯一的，穿过直线的无数多个平面方程都可以构成直线方程组。

* **对称式**

由参数式方程组知，当 $t$ 相等时，有：
$$
\frac{x - x_0}{a} = \frac{y- y_0}{b}= \frac{z- z_0}{c}.
$$

* **两点式**

假设直线穿过两点 $(x_1,y_1)$，$(x_2,y_2)$，由参数方程可得：
$$
\begin{cases}
x = x_1 + at,\\
x_2 = x_1 + at_1.
\end{cases}\&\quad
\begin{cases}
y = y_1 + at,\\
y_2 = y_1 + at_1.
\end{cases}\&\quad
\begin{cases}
z = z_1 + at,\\
z_2 = z_1 + at_1.
\end{cases}\\
\Leftrightarrow \\
\frac{x - x_1}{x_2 - x_1} = \frac{y - y_1}{y_2 - y_1} = \frac{z - z_1}{z_2 - z_1} = \frac{t}{t_1}
$$

同样地，可以用行列式表示：

$$
\begin{vmatrix} x&y&1\\
x_1&y_1&1\\
x_2&y_2&1
\end{vmatrix}
=\begin{vmatrix} y&z&1\\
y_1&z_1&1\\
y_2&z_2&1
\end{vmatrix}
=\begin{vmatrix} z&x&1\\
z_1&x_1&1\\
z_2&x_2&1
\end{vmatrix}
= 0.
$$

* **向量式**

假设直线穿过点 $p_0(x_0,y_0,z_0)$，且方向向量 $\vec{v} = (v_0, v_1, v_2)$，直线的向量方程表示：

$$
p = p_0 + \vec{v}t.\quad (t \in{\mathbb{R}})
$$

* **参数式**

设 $\vec{v} = (a, b, c)$ 由向量方程可以得到:
$$
\begin{cases}
x = x_0 + at,\\
y = y_0 + bt,\\
z = z_0 + ct.
\end{cases}
$$

#### 1.3 直线光栅算法

直线光栅化的目的是把矢量图形转化成光栅图形，光栅图形由像素阵列构成。

* **数字微分分析法（Digital Differential Analyzer）**[<sup>[2]</sup>](#refer-anchor-2)

DDA 算法的基本思想是利用光栅图形的特性从 $x$（或者 $y$）方向以单位间隔（增量 $=1$）发出射线扫描直线，再计算另一方向 $y$（或者 $x$）的增量，在射线与直线的交点位置绘制像素。

直线方程：

$$
y = kx + b.
$$

上面方程式直线的斜率 $k=\frac{\delta{y}}{\delta{x}}$，$\delta{y}$，$\delta{x}$ 分别是 $y$ 和 $x$ 方向上的增量，从斜率公式可以得到：

$$
\begin{cases}
\delta{x} = \frac{\delta{y}}{k},\\
\delta{y} = k\delta{x}.
\end{cases}\tag{1.1}
$$

当斜率 $\vert{k}\vert\leq 1$ 时，选择从 $x$ 轴方向以单位间隔（$\delta{x} = 1$）发出射线，这时计算 $y$ 方向上的增量 $\delta{y} = k\delta{x} = k$。

当斜率 $\vert{k}\vert>1$ 时，选择从 $y$ 轴方向以单位间隔（$\delta{y} = 1$）发出射线，这时计算 $x$ 方向上的增量 $\delta{x} = \frac{\delta{y}}{k} = \frac{1}{k}$。

DDA 算法代码实现如下[<sup>[2]</sup>](#refer-anchor-2)：

```C++
/**
* 像素位置取整。
*/
Vector2i Round(const Vector2i& in) 
{ 
    return in + Vector2i(0.5f, 0.5f); 
}

void DrawPixel(const Vector2i& p) 
{
    ...
}

void DDA(const Vector2i& start, const Vector2i& end)
{
    float dy = end.y - start.y;
    float dx = end.x - start.x;

    float fabsX = fabs(dx);
    float fabsY = fabs(dy);

    Vector2i outPoint = start;
    DrawPixel(Round(outPoint));

    // |k| <= 1 or |k| > 1
    int step = (fabsX >= fabsY) ? fabsX : fabsY;

    // 增量 deltaX, deltaY
    float deltaX = fabsX / (float)step;
    float deltaY = fabsY / (float)step;

    for(int i = 0; i < step; i++)
    {
        outPoint.x += deltaX;
        outPoint.y += deltaY;

        DrawPixel(Round(outPoint));
    }
}
```

DDA 算法的优点是相对直线方程来说速度更快，它利用光栅图形特性（像素阵列）减少了直线方程中的乘法运算（x 或者 y 方向上像素位置间隔是 1，这样可以先确定 x 或者 y 方向上的增量，再求取另一个方向上的增量），缺点是计算过程中的像素位置取整运算会使得光栅图形偏离实际直线，并且取整和浮点运算仍然比较耗时[<sup>[2]</sup>](#refer-anchor-2)。

* **布雷森汉姆算法（Bresenham Algorithm）**[<sup>[2]</sup>](#refer-anchor-2)

Bresenham 算法与 DDA 算法类似，它同样利用了光栅图形的特性，首先假设直线斜率为 $m(0 < m < 1)$， $x$ 方向固定增量为 $1$，当前显示的像素位置是 $(x_k,y_k)$，那么要判定下一个像素的显示位置是 $(x_{k+1},y_k)$，还是 $(x_{k+1},y_{k} + 1)$，判定的依据是什么？下面介绍作为判定依据的决策函数值 $P_k$（decision function）。

接上文，显然，应该选择两个像素当中位置更接近直线的那个绘制。下一个要显示的像素位置其 $x$ 轴坐标为 $x_{k+1}$，代入直线斜截式方程得到：

$$
y = mx_{k+1} + b.\quad
\Leftrightarrow\quad
y = m(x_k+1) + b.\tag{1.2}
$$

计算 $(x_{k+1},y_k)$ 与 $(x_{k+1},y)$ 的距离：

$$
d_{lower} = y - y_k.\tag{1.3}
$$

计算 $(x_{k+1},y_{k} + 1)$ 与 $(x_{k+1},y)$ 的距离：

$$
d_{upper} = y_{k} + 1 - y.\tag{1.4}
$$

式 $\text{(1.3)}$ 减去式 $\text{(1.4)}$ 得到：

$$
d_{lower} - d_{upper}=\\
2y - 2y_k - 1.\tag{1.5}
$$

当 $d_{lower} - d_{upper} \leq{0}$ 时，说明像素位置 $(x_{k+1},y_k)$ 离直线的距离更近，将 $\text{(1.2)}$ 代入式 $\text{(1.5)}$：

$$
d_{lower} - d_{upper}=\\
2m(x_k+1) - 2y_k - 1 + 2b=\\
2mx_k - 2y_k + 2m + 2b - 1.\tag{1.6}
$$

斜率 $m =\frac{\Delta{y}}{\Delta{x}} \Rightarrow \Delta{y} = m\Delta{x}$，式 $\text{(1.6)}$ 两边都乘以 $\Delta{x}$ （消除计算决策函数时的浮点运算）：

$$
P_k=\Delta{x}(d_{lower} - d_{upper})=\\
2\Delta{y}x_k - 2\Delta{x}y_k + \Delta{x}(2m + 2b - 1).\tag{1.7}
$$

由式 $\text{(1.7)}$ 可以得到第 $k+1$ 步的决策函数值：

$$
P_{k+1} = 2\Delta{y}x_{k+1} - 2\Delta{x}y_{k+1} + \Delta{x}(2m + 2b -1).\tag{1.8}
$$

这时，$x_{k+1} = x_{k} + 1$，而 $y_{k+1} - y_k$ 等于 $1$ 或者 $0$ 这取决于 $P_k$ 的符号，式 $\text{(1.8)}$ 减去式 $\text{(1.7)}$ 得：

$$
P_{k+1} - P_k = 2\Delta{y} - 2\Delta{x}(y_{k+1} - y_k).
$$

从以上递归式可以看出，计算决策函数的过程已经没有浮点运算，也没有取整运算。

起始点决策函数值:

$$
\begin{cases}
P_0 = 2\Delta{y}x_0 - 2\Delta{x}y_0 + \Delta{x}(2m + 2b - 1),\\
m = \frac{\Delta{y}}{\Delta{x}},\\
\Delta{x}b = \Delta{x}y_0 - \Delta{y}x_0.
\end{cases}\Rightarrow
P_0 = 2\Delta{y} - \Delta{x}
$$

Bresenham 算法代码实现（斜率 $m(0<m<1)$）：

```C++
/**
* start.x < end.x
*/
void Bresenham(const Vector2i& start, const Vector2i& end)
{
    int deltaX = end.x - start.x;
    int deltaY = end.y - start.y;

    // 起始点决策函数值
    int decision = 2 * deltaY - deltaX;

    // 绘制起始点
    DrawPixel(start);

    int factor = 0;
    int x = start.x + 1;
    int y = start.y;

    // x 轴坐标以步长 1 递进
    for(; x < end.x; x++)
    {
        if(decision < 0)
        {
            // 决策值小于0，y坐标不变
            decision += (2 * deltaY);
        }
        else
        {
            // 决策值大于等同于0，y坐标加1
            y++;
            decision += (2 * deltaY - 2 * deltaX);
        }

        // 绘制像素
        DrawPixel(Vector2i(x, y));
    }
}
```

上文代码只实现了斜率 $0 < m < 1$，且起始点 $x$ 坐标值小于终点 $x$ 坐标值的情形，考虑平面区域对称性，其他情形只需要交换或者改变 $x$ 和 $y$ 方向增量规则即可（1、比如先从 $y$ 方向递进，再判定 $x$ 方向坐标值；2、增量等于 $\pm1$）。

Bresenham 算法相对 DDA 算法速度要快，缺点是线宽是固定的，只支持整型坐标，且光栅图形锯齿比较严重[<sup>[4]</sup>](#refer-anchor-4)。

* **吴小林直线算法**

Wu 画线算法支持反走样，与 Bresenhanm 算法不同的是，它会绘制离直线最近的一个像素对（两个像素），并且计算像素对中两个像素与直线的距离，根据距离的大小给予像素不同的亮度[<sup>[5]</sup>](#refer-anchor-5)。[<sup>[5]</sup>](#refer-anchor-5)文中对线段端点提出特别处理，猜测是因为算法支持线段端点为非整型坐标的缘故。[<sup>[6]</sup>](#refer-anchor-6)提供了 N 种不同语言的代码实现。

* **其他直线光栅算法**

Miloyip 在文中[<sup>[4]</sup>](#refer-anchor-4)提供了多种光栅化线段的算法实现，除 Bresenham 算法外，还包括了单采样、超采样、带符号距离场以及 AABB 优化的带符号距离场等，这几种方法用带面积的形状表示直线，文中[<sup>[4]</sup>](#refer-anchor-4)采用的是胶囊体（capsule），这样可以改变直线的宽度，且支持浮点坐标，也更方便做反走样处理。

文中[<sup>[4]</sup>](#refer-anchor-4)所采用的胶囊体可参见下图:

![capsule-point-intersect](/static/img/21c9f13d0e534cbda8c049618024255a.png)
<p align="center">图 1.1.1 </p>

<!-- <div align=center>
<img src="/static/img/21c9f13d0e534cbda8c049618024255a.png" width=800 />
<p align="center">图 1.1.1 </p>
</div> -->

$A$，$B$ 是胶囊体的两个端点，胶囊体的半径为 $r$，$P_2$，$P_2$，$P_3$，$P_4$，$P_5$ 是胶囊体内外的五个点，

#### 1.4 三角形光栅化

#### 参考

<div id="refer-anchor-1"></div>

[1] [直线-Wiki](https://zh.wikipedia.org/wiki/%E7%9B%B4%E7%BA%BF)

<div id="refer-anchor-2"></div>

[2] 计算机图形学 第4版（OpenGL）

<div id="refer-anchor-3"></div>

[3] [DDA-Wiki](https://en.wikipedia.org/wiki/Digital_differential_analyzer_(graphics_algorithm))

<div id="refer-anchor-4"></div>

[4] [C 语言画线-Miloyip](https://zhuanlan.zhihu.com/p/30553006)

<div id="refer-anchor-5"></div>

[5] [吴小林画线算法-Wiki](https://zh.wikipedia.org/wiki/%E5%90%B4%E5%B0%8F%E6%9E%97%E7%9B%B4%E7%BA%BF%E7%AE%97%E6%B3%95)

<div id="refer-anchor-6"></div>

[6] [吴小林画线算法代码实现](https://rosettacode.org/wiki/Xiaolin_Wu%27s_line_algorithm#C.2B.2B)

<div id="refer-anchor-7"></div>

[7] [Distance Field-Inigo quilez](https://www.iquilezles.org/www/articles/distfunctions/distfunctions.htm)

### 2. 平面

### 3. 多边形网格

### 4. [二次曲面](:/ea3d24c76933456b963183a1bf070d46)

### 5. [圆环面（体）](:/b312d6d704ef422d948b666fb9ac7497)

### 6. [多项式曲线](:/194f3815a52e42119198b99ce7deff46)

### 7. [多项式曲面](:/69d2e6d727ba43b7919aedffcb2bfef3)