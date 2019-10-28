---
layout:        post
title:         "2-SAT 问题"
subtitle:      "2-SAT Problem"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 图论
  - 2-SAT
---

## 定义

设有 $n$ 个布尔变量， $m$ 个约束条件，

要求对这 $n$ 个变量进行赋值，使其满足所有 $m$ 个约束条件。

$[\ 2\ ]$ ：每个约束条件只涉及 $2$ 个布尔变量。

$[\ SAT\ ]$ ： $Satisfiability$ 的缩写，意为可满足性。

举例：

设当前有 $3$ 个布尔变量 $a, b, c$ ， $3$ 个约束条件：

1. $\neg a\or b$ 
2. $a \or b$ 
3. $\neg a\or \neg b$ 

要求对 $a, b, c$ 赋值，满足 $3$ 个约束条件，即：

$(\neg a\or b)\and(a\or b)\and(\neg a\or \neg b) = 1$

## 求解

设 $n$ 个变量编号从 $1$ 到 $n$ ， $n$ 个变量的反值编号从 $n+1$ 到 $2n$ ，

对于 $(a\or b)$ ，转换为 $(\neg a\to b)\and (\neg b\to a)$ ，

对于每个蕴含式，连接相应的边。

对得到的有向图求解强连通分量。

若 $x$ 与 $\neg x$ 在同一强连通分量内，则无解。反之有解。

对于不同的强连通分量 $A, B$ ，设 $A\to B$ ，

可能存在 $x\in A$ 而 $\neg x \in B$ ，此时， $A$ 不能作为合法解，但 $B$ 可作为合法解。

因此，对每个布尔变量赋值时，应选择其所属强连通分量在有向图中拓扑序靠后的取值。

又因为，在使用 $Tarjan$ 算法求解强连通分量的过程中，

强连通分量的编号的大小关系与其拓扑序相反，即编号越大其拓扑序越小，反之同理。

因此，对于每个布尔变量，应选择正反值中，强连通分量编号小的进行赋值。

## 模板

```c++
/* ----- 2-SAT 问题 -----
< 准备 >
    1. 根据约束条件建图 e[] ， u -> v 表示 u 成立时 v 必成立
    .. 下标从 1 开始
    
< 使用 >
    1. 调用 sat.run(n) ，
    .. 返回值 0 表示无解
    .. 返回值 1 表示有解， n 个布尔变量的解储存在 sat.val[] 中
    
< 注意 >
    1. 因为每个布尔变量有两个值，所以点数 N 应开两倍
    .. 点 i+n 表示点 i 的反值
*/

struct two_SAT {
    int val[N], dfn[N], low[N], scc[N], S[N], top, cnt, no;
    void SCC(int u) {
        dfn[u] = low[u] = ++no;
        S[top++] = u;
        for (auto v : e[u]) {
            if (!dfn[v]) {
                SCC(v);
                low[u] = min(low[u], low[v]);
            } else if (!scc[v]) {
                low[u] = min(low[u], dfn[v]);
            }
        }
        if (low[u] == dfn[u]) {
            cnt++;
            do { scc[S[--top]] = cnt; } while (S[top] != u);
        }
    }
    int run(int n) {
        no = top = cnt = 0;
        memset(dfn, 0, sizeof(dfn[0]) * (2*n+1));
        memset(scc, 0, sizeof(scc[0]) * (2*n+1));
        rep(i, 1, 2*n+1) if (!dfn[i]) SCC(i);
        rep(i, 1, n+1) if (scc[i] == scc[i+n]) return 0;
        rep(i, 1, n+1) val[i] = (scc[i] < scc[i+n]);
        return 1;
    }
} sat;
```

