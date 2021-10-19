---
layout: post
title:  "快速排序"
date:   2021-10-19
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

快速排序

- [快速排序](#快速排序)
  - [1. 一次快速排序](#1-一次快速排序)
  - [2. 递归（迭代）](#2-递归迭代)
  - [3. 合并](#3-合并)
  - [参考](#参考)

### 快速排序

怎么样理解快速排序，从名称上来看，其实让人挺懵的，快速？无法从名称中得到上面有效的信息来判断“快速排序”是怎么执行的。"快速排序"是1960年由英国计算机科学家霍尔发明的，它的核心思想是“分治法”，将待排序集合划分为多个子集，使子集有序，而后合并子集得到有序集合。

分治法即“分而治之”，它将一个大的难以直接解决的问题划分为多个小规模的子问题，直到子问题可以简单的直接求解，原问题的解即各个子问题解的合并。因此，“分治法”通常有三个步骤[<sup>[1]</sup>](#refer-anchor-1)：

- 分解：将原问题分解为若干个规模较小，相对独立，与原问题形式相同的子问题。
- 解决：若子问题规模较小且易于解决时，则直接解。否则，递归地解决各子问题。
- 合并：将各子问题的解合并为原问题的解。

下面，参照“分治法”的三个步骤讨论快速排序是怎么执行的。

#### 1. 一次快速排序

假设有一个数组：[5, 4, 3], 以 4 为基准数值将其划分得到两个子数组 [3] 和 [5]，显然子数组 [3] 和 [5] 都只有一个元素，它们是最小的待排序子数组，可以直接判定它们必定是有序的。怎么样合并子数组 [3] 、[5] 以及基准数 4 得到完整的有序数组呢？以 4 为基准数值（从小到大排序），通过交换操作，将小于它的子数组放在左边，将大于它的子数组放在右边，这样就保证了：左子数组 < 基准数值 < 右子数组，如果左子数组和右子数组分别是有序的，最终合并后的数组即是有序的。

因此，快速排序每一次划分集合的时候，只需要保证：左子集 < 基准值 < 右子集。

通常一般的快速排序都会选择区间内第一个元素为基准值，划分集合代码：

```C++
int Partion(vector<int> &input, int start, int end) {
  int pivotIndex = start;
  int pivot = input[pivotIndex];
  while (start < end) {
      while (input[end] >= pivot && start < end) {
          --end;
      }

      while (input[start] <= pivot && start < end) {
          ++start;
      }
      
      if (start < end) {
          int temp = input[start];
          input[start] = input[end];
          input[end] = temp;
      }
  }
  
  // 当 start == end 时，基准值的位置
  input[pivotIndex] = input[start];
  input[start] = pivot;
  
  return start;
}
```

#### 2. 递归（迭代）

对于集合 $A$ 来说，每一次划分都只能保证 ${A}$ 子集之间是有序的 ${A_{left}} < A_0 < A_{right}$ ，并不能保证 $A_{left}$ 与 $A_{right}$ 子集本身是有序的，因此，需要递归的不断对 $A$ 左右子集划分，直到子集中只有一个元素为止（递归划分结束条件）。

递归划分实现：

```C++
void QuickSort(vector<int> &input, int start, int end) {
    if (start >= end) {
        return;
    }

    int split = Partion(input, start, end);
    QuickSort(input, start, split -1);
    QuickSort(input, split + 1, end);
}
```

嗯嗯！我们都已经听说了，递归是利用堆栈来实现的，每当函数调用自身时，堆栈就会增加一层，每当函数返回时，堆栈就会减少一层。

递归过程中堆栈到底存了些什么？

递归函数调用过程中堆栈存储了函数执行上下文。以上面 QuickSort 递归实现为例，我们可以观察到，每一次递归调用时，split、start 和 end 变量值都是不一样的，每一次调用我们都需要在堆栈中存储一份这些变量值，多少次调用便存储多少份。

迭代划分实现：

```C++
struct Range {
    int start;
    int end;

    Range(int s, int e) 
    : start(e)
    , end(e) {}
};

void QuickSort(vector<int> &input) {
    vector<Range> ranges;
    ranges.push_back(Range(0, input.size() - 1));

    while (ranges.size() > 0) {
        auto &range = ranges.back();
        if (range.start >= range.end) {
            continue;
        }
        ranges.pop_back();

        int split = Partion(input, range.start, range.end);
        ranges.push_back(Range(range.start, split - 1));
        ranges.push_back(Range(split + 1, range.end));
    }
}
```

#### 3. 合并

快速排序划分时的交换操作，已经能够确保子集之间的排序关系（与归并排序不同，归并排序不能提前确定子集之间的排序关系，因此需要合并操作），因此不需要再执行合并操作。

#### 参考

<div id="refer-anchor-1"></div>

[1] [分治法-维基百科](https://zh.wikipedia.org/zh-hans/%E5%88%86%E6%B2%BB%E6%B3%95)

<div id="refer-anchor-2"></div>

[2] [ OI Wiki 递归 & 分治](https://oi-wiki.org/basic/divide-and-conquer/)
