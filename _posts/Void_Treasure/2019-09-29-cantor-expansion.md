---
layout:        post
title:         "康托展开 & 逆康托展开"
subtitle:      "Cantor Expansion & Reverse Cantor Expansion"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 组合数学
  - 康托展开
---

### 康托展开

求给定长度为 $n$ 的全排列 $P$ 在所有长度为 $n$ 的全排列中的字典序排名。

即，将全排列映射为一个数。

方法：

1. 问题可转化为：有多少个比当前全排列 $P$ 字典序小的全排列
2. 枚举 $i$ ，求有多少个全排列 $Q_i$ ，它们的前 $i-1$ 个数与 $P$ 相同，且字典序比 $P$ 小
3. 对于 $Q_i$ ，其第 $i$ 位要与 $P$ 不同，则只能在 $P$ 的 $i+1$ 到 $n$ 位中，选择字典序比 $P[i]$ 小的数
4. 当选择好第 $i$ 位的数后，剩余 $n-i$ 个数即可任意排列，方案即为 $(n-i)!$
5. 最后将求得总数 $+ 1$ ，即为 $P$ 的排名（从 $1$ 开始）

$$
ans = 1+\sum_{i=1}^nA[i]\times(n-i)!\\
A[i] = \sum_{j=i+1}^n[a[j] < a[i]]
$$

```c++
int Cantor_Expansion(int a[], int n) {
    seg.build(1, n, 1);
    int res = 1, jc = 1;
    per(i, 1, n+1) {
        res += jc * seg.sum(a[i], 1, n, 1);
        jc *= n - i + 1;
        seg.upd(a[i], 1, n, 1);
    }
    return res;
}
```



---



### 逆康托展开

给定长度 $n$ 与排名 $x$ ，求对应的全排列 $P$ 

方法：

1. 根据康托展开的公式，逆序推导。
2. 先将 $x - 1$
3. 从 $i=1$ 开始，$y = x / (n-i)!$ ，表示 $P[i]$ 大于后 $(n-i)$ 个数中的 $y$ 个数
4. 因为 $A[i] \le (n-i)$ ，所以保证推导过程中整除和取余不会影响其他位。
5. 根据 $y$ 求出 $P[i]$ ，即未出现过的第 $y+1$ 小的数
6. 余数进入下一次计算

```c++
void Reverse_Cantor_Expansion(int x, int n, int b[]) {
    x--;
    seg.build(1, n, 1);
    int y, jc = 1; rep(i, 1, n+1) jc *= i;
    rep(i, 1, n+1) {
        jc /= n - i + 1;
        y = x / jc; x %= jc;
        b[i] = seg.qry(y + 1, 1, n, 1);
        seg.upd(b[i], 1, n, 1);
    }
}
```



---



### 线段树部分

```c++
#define ls rt << 1
#define rs rt << 1 | 1
#define lson l, m, ls
#define rson m + 1, r, rs
int t[N<<2];
struct Seg {
    void build(int l, int r, int rt) {
        if (l == r) { t[rt] = 0; return ; }
        int m = (l + r) >> 1;
        build(lson); build(rson);
        t[rt] = t[ls] + t[rs];
    }
    int qry(int y, int l, int r, int rt) {
        if (l == r) return l;
        int m = (l + r) >> 1, len = m - l + 1;
        if (len - t[ls] >= y) return qry(y, lson);
        return qry(y - (len - t[ls]), rson);
    }
    void upd(int p, int l, int r, int rt) {
        if (l == r) { t[rt]++; return ; }
        int m = (l + r) >> 1;
        if (p <= m) upd(p, lson); else upd(p, rson);
        t[rt] = t[ls] + t[rs];
    }
    int sum(int p, int l, int r, int rt) {
        if (l == r) return t[rt];
        int m = (l + r) >> 1;
        if (p <= m) return sum(p, lson);
        return t[ls] + sum(p, rson);
    }
} seg;
```

