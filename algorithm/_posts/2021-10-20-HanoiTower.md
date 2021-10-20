---
layout: post
title:  "经典汉诺塔问题"
date:   2021-10-20
categories: 
    - algorithm
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

经典汉诺塔问题

- [1. 问题描述](#1-问题描述)
- [2. 分解](#2-分解)
- [3. 代码实现](#3-代码实现)
- [参考](#参考)

#### 1. 问题描述

链接：https://www.nowcoder.com/questionTerminal/5584989e7c4c442ca5f15c1184075b25

设有n个大小不等的中空圆盘，按照从小到大的顺序迭套在立柱A上（最上面是1号盘子），另有两根立柱B和C。现要求把全部圆盘从A柱（源柱）移到C柱（目标柱），移动过程中可借助B柱（中间柱）。移动时有如下的要求：
① 一次只许移动一个盘；
② 不允许把大盘放在小盘上边；
③ 可使用任意一根立柱暂存圆盘

#### 2. 分解

如果我们直接动手操作的话，一步一步地我们或许可以很好的解决汉诺塔问题，但是，怎么样让计算机去理解我们的操作，形成规律的计算方法，我觉得这是难点。

三根柱子 $A,B,C$，初始时所有的盘子按照从下到上，从大到小的顺序堆放在 A 柱上。

1. 假设 A 柱只有一个盘子时，那么很简单，我们只需要将盘子从 A 柱移动到 C 柱上即可。
2. 假设 A 柱上有两个盘子时，那么我们需要一个辅助柱，首先将小的盘子从 A 柱上移动到 B 柱上，接下来发现 A 柱上只有一个盘子了，这时候我们只需要把剩下的盘子直接从 A 柱移动到 C 柱上即可，再接着把 B 柱上的盘子移动到 C 柱。

参照假设 2，我们可以把 ${n}$ 个盘子分成两部分 ${(a_1) 和 (a_2, a_3,...,a_n)}$，如果我们可以成功的把 $(a_2, a_3,...,a_n)$ 盘子放到 B 柱上，那么 A 柱就只剩下一个盘子 $a_1$，这时候问题就变成了假设 1，可以直接移动盘子。

接下来，B 柱上的 ${n - 1}$ 个盘子该怎么处理呢？同理我们把这 ${n - 1}$ 个盘子也分成两部分 ${(b_2) 和 (b_3, b_4,...,b_n)}$，以 A 柱为辅助柱，将 $(b_3, b_4,...,b_n)$ 移动到 A 柱，再将 ${b_2}$ 从 B 柱直接移动到 C 柱。

注意，上面假设中将 $(a_2, a_3,...,a_n)$ 从 A 柱移动到 B 柱，目标柱是 B 柱，辅助柱是 C。将 $(b_3, b_4,...,b_n)$ 从 B 柱移动到 A 柱，目标柱是 A 柱，辅助柱是 C。

当 A 柱或者 B 柱上还有两个或以上盘子时，只需要重复以上步骤假设即可。

#### 3. 代码实现

递归：

```C++
void HanoiTower(int n, string source, string helper, string target) {
    if (n == 1) {
        cout << n <<  " source to target" << endl;
        return;
    }

    // 首先将 n - 1 个盘子从 A 柱移动到 B 柱，B 柱是目标柱，辅助柱是 C 柱
    HanoiTower(n-1, source, target, helper);

    // n - 1 个盘子已经移动到 B 柱，现在将 A 柱上剩下的盘子移动到 C 柱上
    cout << n <<  " source to target" << endl;

    // 现在 B 柱变成了初始柱，上面有 n - 1 个盘子，目标柱是 C, 辅助柱是 A
    HanoiTower(n-1, helper, source, target);
}
```

迭代：

```C++
```

#### 参考

<div id="refer-anchor-1"></div>

[1] [分治法-维基百科](https://zh.wikipedia.org/zh-hans/%E5%88%86%E6%B2%BB%E6%B3%95)

<div id="refer-anchor-2"></div>

[2] [ OI Wiki 递归 & 分治](https://oi-wiki.org/basic/divide-and-conquer/)
