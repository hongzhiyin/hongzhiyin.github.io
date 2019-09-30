---
layout:        post
title:         "欧拉定理 & 扩展欧拉定理"
subtitle:      "Euler's theorem & Extended Euler's theorem"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 数论
  - 欧拉函数
---

### 欧拉定理

当 $a,\ m \in \Z$ ，且 $gcd(a,\ m) = 1$ 时，有：
$$
a^{\varphi(m)}\equiv 1\ (mod\ m)
$$
则：
$$
a^b\equiv a^{b\ mod\ \varphi(m)}\ (mod\ m)
$$


### 扩展欧拉定理

当 $a,\ m \in \Z$ 时，有：
$$
a^b \equiv
\begin{cases}
a ^ b & b < \varphi(m) \\
a^{b\ mod\ \varphi(m)\ +\ \varphi(m)} & b \ge \varphi(m)
\end{cases}
(mod\ m)
$$


```c++
/* ----- 扩展欧拉定理 -----
< 内容 >
    Calculate a ^ b (mod m)
        if (b < phi(m)) ans = qpow(a, b, m);
        else ans = qpow(a, b % phi(m) + phi(m), m);
< 准备 >
    1. 底数 a ，指数 b （指数很大，存在字符串 s[] 中），模数 m
< 使用 >
    1. 调用 Euler_qpow(a, s, m) 得 a ^ b (mod m)
*/

ll Euler_qpow(ll a, char s[], ll m) {
    int len = strlen(s), ok = 0; ll b = 0, p = phi(m);
    rep(i, 0, len) {
        b = b * 10 + s[i] - '0';
        if (b >= p) b %= p, ok = 1;
    }
    if (ok) return qpow(a, b + p, m);
    return qpow(a, b, m);
}
```

