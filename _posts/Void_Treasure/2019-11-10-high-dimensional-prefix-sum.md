---
layout:        post
title:         "高维前缀和"
subtitle:      "High-Dimensional Prefix Sum"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 高维前缀和
---





### 一维前缀和

```c++
pre[i] = pre[i-1] + a[i];
```



### 二维前缀和

**容斥原理**

```c++
pre[i][j] = pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1] + a[i][j];
```

**求两次前缀和**

```c++
rep(i, 0, n) rep(j, 0, m) pre[i][j] = a[i][j];
rep(k, 0, 2) {
    rep(i, 0, n) rep(j, 0, m) {
        if (k) pre[i][j] += pre[i-1][j];
        else pre[i][j] += pre[i][j-1];
    }
}
```



### k 维前缀和

**求 k 次前缀和**

```c++
rep(i1, 0, d1) ... rep(ik, 0, dk) pre[i1]..[ik] = a[i1]..[ik];
rep(d, 1, k+1) {
    rep(i1, 0, d1) ... rep(ik, 0, dk) {
        switch (d) {
            case 1 : pre[i1]..[ik] += pre[i1-1]..[ik]; break;
            ...
            case k : pre[i1]..[ik] += pre[i1]..[ik-1]; break;
        }
    }
}
```



### 原理

设一个 $k$ 维空间中，每个点的点权初始都表示自己的点权。

现求 $k$ 维前缀和，使得每个点 $V$ 的点权表示所有 各维坐标皆小于等于 $V$ 的点 的点权之和。

1. 枚举 $k$ 个维度

2. 对于每一个维度 $i$ ，从小到大遍历点集，对于每个点 $V$ 

   $V[x_1, .. , x_i , .. , x_k] \mathrel{+}= V[x_1, .. , x_i - 1, .. , x_k]$ 

这样， $k$ 个维度都枚举完后，每个点 $V$ 的点权即表示 $k$ 维前缀和。



### 用二进制表示的子集前缀和

`pre[mask]` ： `mask` 表示的子集的所有子集之和。

```c++
if (x | mask == mask) pre[mask] += pre[x];
```

可看作是有 $n$ 个维度，每个维度的值域只有 $0$ 和 $1$ 的高维前缀和

```c++
rep(i, 0, n) rep(mask, 1, 1 << n) if (mask >> i & 1)
    pre[mask] += pre[mask ^ 1 << i];
```

