---
layout:        post
title:         "仙人掌图"
subtitle:      "Cactus Graphs"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 图论
  - 仙人掌图
---

## 定义

无自环、无重边的无向连通图，任意一条边，最多属于一个简单环。



## 环

仙人掌图中的每个环都是一个点双连通分量。

可用 $dfs$ 找出环的个数以及每个环的大小。

```c++
int dep[N];
void dfs(int u, int f=0) {
    dep[u] = dep[f] + 1;
    for (auto v : e[u]) if (v != f) {
        if (!dep[v]) {
            dfs(v, u);
        } else if (dep[u] > dep[v]) {
            Circle.pb(dep[u] - dep[v] + 1);
        }
    }
}
```

也可用 $Tarjan$ 算法求出所有的点双连通分量，即所有的环，包括环的个数、每个环的大小以及每个环都由哪些点构成。

