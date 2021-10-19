---
layout: post
title:  "归并排序"
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

归并排序

- [归并排序](#归并排序)
  - [1. 分解](#1-分解)
  - [2. 解决](#2-解决)
  - [3. 合并](#3-合并)
  - [4. 代码实现](#4-代码实现)
  - [参考](#参考)

### 归并排序

与快速排序一样，归并排序的核心思想也是“分治法”[<sup>[1]</sup>](#refer-anchor-1)。

#### 1. 分解

通常归并排序将集合从中间分解形成两个子集。

#### 2. 解决

如果子集中只有一个元素，那么该子集本身就是有序的，否则将继续分解子集。

#### 3. 合并

不断分解后，假设集合 $A$ 有两个有序子集 $A_{left}$ 和 $A_{right}$，通过步骤 1 和 2 并不能确定子集 $A_{left}$ 与 $A_{right}$ 的关系，子集 $A_{left}$ 中的元素可能大于或者小于子集 $A_{right}$ 中的元素，因此，需要逐个比较两个子集中的元素，并将其合并到新的集合中去。

#### 4. 代码实现

递归:

```C++
void Merge(vector<int> &array, int start, int mid, int end) {
    vector<int> leftArray(array.begin() + start, array.begin() + mid);
    vector<int> rightArray(array.begin() + mid + 1, array.begin() + end);

    // 插入最大数值，防止下面循环时访问越界
    leftArray.insert(leftArray.end(), numeric_limits<int>::max());
    rightArray.insert(rightArray.end(), numeric_limits<int>::max());

    int left = 0;
    int right = 0;

    int index = start;
    while (index <= end) {
        if (leftArray[left] <= rightArray[right]) {
            array[index++] = leftArray[left++];
        } else {
            array[index++] = rightArray[right++];
        }
    }
}

void MergeSort(vector<int> &array, int start, int end) {
    if (start >= end) {
        return;
    }

    int mid = (start + end) / 2;
    MergeSort(array, start, mid);
    MergeSort(array, mid + 1, end);
    Merge(array, start, mid, end);
}
```

迭代：
归并排序的迭代实现需要自下而上反推，将 $n$ 个元素的待排序数组看作 $n$ 个只有一个元素的子数组，这样我们可以按照 $1、2、4、8...$ 的步长合并数组。

```C++
void MergeSort(vector<int> &input) {
    vector<int> temp(input.begin(), input.end());
    
    for (int step = 1, size = input.size(); step < size; step <<= 1) {
        for (int i = 0; i < size; i += step + step) {
            int start1 = i;
            int mid = min(i + step - 1, size - 1);
            int start2 = mid + 1;
            int end = min(i + step + step - 1, size - 1);
            int index = start1;
            while (start1 <= mid &&
                   start2 <= end) {
                if (input[start1] <= input[start2]) {
                    temp[index++] = input[start1++];
                } else {
                    temp[index++] = input[start2++];
                }
            }
            
            while (start1 <= mid) {
                temp[index++] = input[start1++];
            }

            while (start2 <= end) {
                temp[index++] = input[start2++];
            }
        }
        
        input.assign(temp.begin(), temp.end());
        
        for (auto &e : input) {
            cout << e << ",";
        }
        cout << endl;
    }
}
```

#### 参考

<div id="refer-anchor-1"></div>

[1] [分治法-维基百科](https://zh.wikipedia.org/zh-hans/%E5%88%86%E6%B2%BB%E6%B3%95)

<div id="refer-anchor-2"></div>

[2] [ OI Wiki 递归 & 分治](https://oi-wiki.org/basic/divide-and-conquer/)
