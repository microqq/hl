---
layout: post
title:  "基本3D图形（二）：平面"
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

基本3D图形：平面

- [2. 平面](#2-平面)
  - [2.1 平面方程](#21-平面方程)
  - [2.2 绘制平面](#22-绘制平面)
  - [参考](#参考)

### 2. 平面

直观来看，平面可以是一个平坦，无限宽大且没有厚度的一个东西，数学上，平面是一个基本的二维数学对象[<sup>[1]</sup>](#refer-anchor-1)。

#### 2.1 平面方程

- **一般式**

在“直线”那一章中使用过平面的一般方程式表示直线：

$$
Ax + By + Cz + D = 0.
$$

这是一个线性方程，为求解 $x,y,z$ 三个未知数，写一个方程组如下：

$$
\begin{cases}
Ax + By + Cz = -D,\\
0x + 0y + 0z = 0,\\
0x + 0y + 0z = 0.
\end{cases}\tag{2.1}
$$

可以得到方程组的增广矩阵为：

$$
\begin{bmatrix}
A&B&C&D\\
0&0&0&0\\
0&0&0&0\\
\end{bmatrix}
$$

观察增广矩阵的系数部分的主元列，可知方程 $\text{(2.1)}$ 有两个自由变量，用向量方式表示方程的解：

$$
X=
\begin{bmatrix}
x\\
y\\
z
\end{bmatrix}=
\begin{bmatrix}
-y\frac{B}{A}-z\frac{C}{A}-\frac{D}{A}\\
y\\
z
\end{bmatrix}=y
\begin{bmatrix}
-\frac{B}{A}\\
1\\
0
\end{bmatrix}+z
\begin{bmatrix}
-\frac{C}{A}\\
0\\
1
\end{bmatrix}+
\begin{bmatrix}
-\frac{D}{A}\\
0\\
0
\end{bmatrix}\tag{2.2}
$$

上式最右边部分式非齐次线性方程组的一个特解，观察下式：

$$
y
\begin{bmatrix}
-\frac{B}{A}\\
1\\
0
\end{bmatrix}+z
\begin{bmatrix}
-\frac{C}{A}\\
0\\
1
\end{bmatrix}
$$

这是两个线性不相关向量的线性组合，它可以确定一个平面，而上面非齐次线性方程组的特解则可以理解为一个平移向量：

$$
T=
\begin{bmatrix}
-\frac{D}{A}\\
0\\
0
\end{bmatrix}
$$

上面从线性方程组的角度理解了平面方程一般式，下面从几何角度来看。假设原点是 $O$，原点外有一点 $N(A,B,C)$，向量 $\vec{n}=\vec{ON}$，平面上的任一点表示为 $P(x,y,z)$，向量 $\vec{p}=\vec{OP}$，线段 $ON$ 穿过平面，与平面的交点为 $Q$，我们知道向量 $\vec{p}$ 在 $\vec{n}$ 上的投影向量为：

$$
p_{\|} = n\frac{p\cdot{n}}{\vert{n}\vert^2}.\tag{2.3}
$$

投影向量的模（有符号）为:

$$
\pm\vert{p_{\|}}\vert = \frac{p\cdot{n}}{\vert{n}\vert}.\tag{2.4}
$$

设 $-D = \pm\vert{p_{\|}}\vert\vert{n}\vert$，结合 $\text{(2.4)}$ 可得：

$$
p\cdot{n} + D = 0\quad\Leftrightarrow\quad{Ax+By+Cz+D=0}.
$$

- **点法式**

已知平面上一点与法向量：

$$
Ax+By+Cz=Ax_0+By_0+Cz_0.
$$

#### 2.2 绘制平面

怎么使用现代图形程序接口 OpenGL 或者 Direct3D 绘制一个平面，如果是一个有限宽度的平面，那么比较简单，输入三角形面片，编写简单的顶点和像素着色器程序即可完成。无限平面无法定义完整的三角网格，那么如何绘制一个无限宽度的平面呢？

平面最终会转换成光栅图形在显示器上显示，我们可以从光栅图形反推平面，看下图：
<!-- ![Infinite-plane](resources/91c6c612c28c477492e8f746f3db106d.png) -->

<div align=center>
<img src="/hl/static/img/infinite-plane.png" width=800 />
<p align="center">图 1 </p>
</div>

图中有一个摄像机视景体，从摄像机的位置发出一条射线分别与近截面相交于点 $P$，与平面相交于点 $R$，与远截面相交于点 $T$。一般情况下绘制图形，会准备好顶点以及三角网格数据，但是平面不能这样做，因为平面本身是无限宽的，无法输出完整的图形数据，所以，我们可以从平面上的点投影到近截面的点 $P_{proj}$ 来绘制平面。以 OpenGL 为例，规范立方体（Canonical View Volnme）的坐标范围是 $-1\leq{x,y,z}\leq{1}$，近截面点 $P$ 投影到规范立方体中，假设投影点坐标 $P_{proj}$ 为 $(x_0,y_0,-1)$，同理，远截面点 $T$ 投影点坐标 $T_{proj}$ 为 $(x_0,y_0,1)$。使用视图投影的逆矩阵乘以投影点坐标 $P_{proj}$，可以得到近截面点 $P$ 的坐标，同理，可以得到远截面点 $T$ 的坐标。

$$
P = mvp_{inv}\times{P_{proj}},\\
T = mvp_{inv}\times{T_{proj}}.
$$

射线方向为 $\vec{PT}$，可得射线方程为：

$$
Ray = P + t\vec{PT}.
$$

已知射线方程，联立平面方程可以得到射线与平面交点 $R$ 坐标：

$$
\begin{cases}
Ray = P + t\vec{PT},\\
Ax+By+Cz+D=0.
\end{cases}
$$

如果 $0 < t < 1$，表示射线与平面存在交点，否则，交点 $R$ 不存在。

绘制平面顶点着色器程序代码如下：

```GLSL
#version 330 core
#extension GL_ARB_shading_language_include : require

#include </GLSLIncludes/vertexLayout.glsl>

//ray start point
out vec3 nearPosition;
//ray end point
out vec3 farPosition;

uniform mat4 mvp;

void main()
{
    mat4 mvpInverse = inverse(mvp);
    vec4 invPosition = (mvpInverse * vec4(vertexPos.xy, -1.0f, 1.0f));
    nearPosition = invPosition.xyz / invPosition.w;

    invPosition = (mvpInverse * vec4(vertexPos.xy, 1.0f, 1.0f));
    farPosition = invPosition.xyz / invPosition.w;

    gl_Position = vec4(vertexPos.xy, -1.0f, 1.0f);
}
```

像素着色器程序代码如下：

```GLSL
#version 330 core
#extension GL_ARB_conservative_depth : enable

layout (location = 0) out vec4 fragcolor;
layout (depth_greater) out float gl_FragDepth;

//直线向量方程: P0 + Vt, eg: P0 = nearPosition V = farPosition - nearPosition
in vec3 nearPosition;
in vec3 farPosition;

//平面方程: Ax + By + Cz + D = 0
//直线与平面相交于 P1, compute parameter t = (-D - (A, B, C) * P0) / (A, B, C) * V
uniform vec4 planeParam;
uniform mat4 mvp;
uniform mat3 rotateMat;

uniform sampler2D mainTex;

void main()
{
  vec3 planeNormal = planeParam.xyz;
  vec3 rayDirection = farPosition - nearPosition;

  float dotPR = dot(planeNormal, rayDirection);
  if(abs(dotPR) < 0.00001f)
  {
    discard;
  }

  float t = (-planeParam.w - dot(planeNormal, nearPosition)) / dotPR;
  if(t >= 0.99f || t < 0.0f)
  {
    discard;
  }
  vec3 intersect = nearPosition + t * rayDirection;
  vec4 intersectProj = mvp * vec4(intersect, 1.0f);

  float near = gl_DepthRange.near;
  float far = gl_DepthRange.far;
  float depth = (intersectProj.z / intersectProj.w + 1.0f) * 0.5f * (far - near) + near;
  gl_FragDepth = depth;

  //绘制纹理
  //先将平面移动到原点
  vec3 pos = intersect - planeNormal * (-planeParam.w);
  //再将平面旋转到 xz 平面
  pos = rotateMat * pos;
  vec2 texcoord = (pos.xz) / 32.0f;
  fragcolor = texture(mainTex, texcoord) * vec4(0.7f, 0.7f, 0.7f, 1.0f);
}
```

#### 参考

<div id="refer-anchor-1"></div>

[1] [平面-Wiki](https://zh.wikipedia.org/wiki/%E5%B9%B3%E9%9D%A2_(%E6%95%B0%E5%AD%A6))

<div id="refer-anchor-2"></div>

[2] Philip Schneider, David H. Eberly, Geometric tools for computer graphics

<div id="refer-anchor-3"></div>

[3] [possumwood,Infinite-ground-plane-using-GLSL-shaders](https://github.com/martin-pr/possumwood/wiki/Infinite-ground-plane-using-GLSL-shaders)
