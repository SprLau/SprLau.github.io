---
layout:     post
title:      Algorithm Classics
subtitle:	Exemplary Problems Solved With Various Basic Algorithms
date:       2021-07-13
author:     Springs Lau
header-img: img/post-web.jpg
catalog: true
tags:
    - Blog

---

# Algorithm Classics

## 1. 方格取数

> 动态规划，递归，费用流

### 题目描述

设有`N * N`的方格图（`N ≤ 9`），我们将其中的某些方格中填入正整数，而其他的方格中则放入数字`0`。如下所示：

```
A
 0  0  0  0  0  0  0  0
 0  0 13  0  0  6  0  0
 0  0  0  0  7  0  0  0
 0  0  0 14  0  0  0  0
 0 21  0  0  0  4  0  0
 0  0 15  0  0  0  0  0
 0 14  0  0  0  0  0  0
 0  0  0  0  0  0  0  0
                         B
```

某人从图的左上角的`A`点出发，可以向下行走，也可以向右行走，直到到达右下角的`B`点。在走做的路上，他可以取走方格中的数（取走后的方格中将变为数字`0`）。

此人从`A`点到`B`点共走两次，试找出2条这样的路径，使得取得的数之和为最大。

### 输入格式

输入的第一行为一个整数N（表示`N * N`的方格图），接下来的每行有三个整数，前两个表示位置，第三个数为该位置上所放的数。一行单独的`0`表示输入结束。

### 输出格式

只需输出一个整数，表示2条路径上取得的最大的和。

### 输入输出样例

> 输入

```
8
2 3 13
2 6  6
3 5  7
4 4 14
5 2 21
5 6  4
6 3 15
7 2 14
0 0  0
```

> 输出

```
67
```

### 说明/提示

NOIP 2000 提高组第四题

### 参考题解

```c++
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<string.h>

using namespace std;

#define MAX     15

int n, x, y, w;
int store[MAX][MAX], sum[MAX][MAX][MAX][MAX];

void input() {
    memset(store, 0, sizeof(store));
    memset(sum, 0, sizeof(sum));
    cin >> n;
    for (; ;) {
        cin >> x >> y >> w;
        if (x == 0 && y == 0 && w == 0) {
            break;
        }
        store[x][y] = w;
    }
}

void process() {
    for (int i = 1; i <= n; i++) {
        for (int ii = 1; ii <= n; ii++) {
            for (int iii = 1; iii <= n; iii++) {
                for (int iiii = 1; iiii <= n; iiii++) {
                    sum[i][ii][iii][iiii] = 
                        max(max(sum[i - 1][ii][iii - 1][iiii], sum[i - 1][ii][iii][iiii - 1]), 
                            max(sum[i][ii - 1][iii - 1][iiii], sum[i][ii - 1][iii][iiii - 1])) 
                        + store[iii][iiii] + store[i][ii];
                    if (i == iii && ii == iiii) {
                        sum[i][ii][iii][iiii] -= store[i][ii];
                    }
                }
            }
        }
    }
    cout << sum[n][n][n][n];
}

int main() {
    input();
    process();

    return 0;
}
```

