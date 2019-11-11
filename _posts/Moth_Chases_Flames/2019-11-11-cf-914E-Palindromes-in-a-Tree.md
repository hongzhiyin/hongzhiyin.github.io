---
layout:        post
title:         "CF 914E Palindromes in a Tree"
subtitle:      "CF 914E Palindromes in a Tree"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
tags:
  - 逐火之蛾
  - 点分治
---



**题意**

一棵 $n$ 个节点的树，每个节点上有一个字母，问：

对于每一个点，经过这个节点的所有路径中，重新排列路径上的字母可以使其变成回文串的路径有多少条。

$2 \le n \le 2 \cdot 10^5$

$a \le s[i] \le t$ 

**题解**

重新排列，因此对于每一条路径，

只要检查出现的所有字母是否至多只有一个字母出现奇数次。

因为字母种类只有 $20$ 种，因此可用整型记录状态。

对于一棵树：

1. 统计从根节点出发的所有路径，对应状态出现次数记录在整形数组中。

2. 对于每一棵子树

   1. 首先清除从根节点到该子树的路径提供的贡献。
   2. 遍历该子树每一个节点，统计从该节点出发到根或其他子树的路径。
   3. 还原从根节点到该子树的路径提供的贡献。

3. 在上述过程中，会被重复计算的路径有：

   1. 从根出发的路径 与 从子树节点出发到根的路径。
   2. 从子树节点出发到另一棵子树节点的路径。

4. 因此这部分答案要除以 $2$

5. 但是，这部分答案中包含了 只有一个根节点的路径，

   因为除以 $2$ 的原因被扣掉了，因此最终答案要重新加 $1$ 

6. 在统计子树节点 $u$ 出发经过根到达另一棵子树节点 $v$ 的路径时

   1. 实际上也要把答案加入到 $ans[u]$ 中，

      因为待会就会把根删除，所以之后就统计不到该条路径了。

   2. 且这部分答案的重复计算恰好分别贡献给 $u$ 和 $v$ ，因此不用除以 $2$ 

   3. 对于 $u$ 的子节点的同样经过根节点的路径，也要贡献给 $ans[u]$ 

用点分治保证复杂度。

**代码**

```c++
const int N = 2e5+7;

// ------- 变量 ------- //

int n;
vi e[N];
char s[N];
ll cnt[2000000], ans[N];

struct Centroid_Decomposition {
    int vis[N], sz[N];
    void init(int n) {
        memset(vis, 0, sizeof(vis[0]) * (n+1));
        memset(cnt, 0, sizeof(cnt[0]) * (n+1));
    }
    void Gravity(int u, int f, int n, int &rt) {
        sz[u] = 1;
        for (auto v : e[u]) if (!vis[v] && v != f) { Gravity(v, u, n, rt); sz[u] += sz[v]; }
        if (!rt && sz[u] * 2 > n) rt = u;
    }
    void run(int u) {
        int rt;
        Gravity(u, 0, 0, rt);
        Gravity(u, 0, sz[u], rt = 0);
        vis[rt] = 1;
        calc(rt);
        for (auto v : e[rt]) if (!vis[v]) run(v);
    }

    // ----- //
    void dfs(int u, int f = 0, int mask = 0, int val = 1) {
        mask ^= 1 << s[u];
        cnt[mask] += val;
        for (auto v : e[u]) if (!vis[v] && v != f) dfs(v, u, mask, val);
    }
    ll work(int u, int f, int mask = 0) {
        mask ^= 1 << s[u];
        ll sum = cnt[mask];
        rep(i, 0, 20) sum += cnt[mask ^ 1 << i];
        for (auto v : e[u]) if (!vis[v] && v != f) sum += work(v, u, mask);
        ans[u] += sum;
        return sum;
    }
    void calc(int u) {
        dfs(u);
        ll sum = cnt[0];
        rep(i, 0, 20) sum += cnt[1 << i];
        for (auto v : e[u]) if (!vis[v]) {
            dfs(v, u, 1 << s[u], -1); 
            sum += work(v, u);
            dfs(v, u, 1 << s[u], 1);
        }
        ans[u] += sum / 2;
        dfs(u, 0, 0, -1);
    }
    // ----- //
} obj;

// ------- 函数 ------- //

void Init() {
    scanf("%d", &n);
    memset(ans, 0, sizeof(ans));
    rep(i, 1, n+1) e[i].clear();
    rep(i, 1, n) {
        int u, v; scanf("%d%d", &u, &v);
        e[u].pb(v); e[v].pb(u);
    }
    scanf("%s", s + 1);
}

int Solve() {
    int len = strlen(s + 1);
    rep(i, 1, len+1) s[i] -= 'a';
    obj.init(n);
    obj.run(1);
    rep(i, 1, n+1) printf("%lld%c", ans[i] + 1, " \n"[i==n]);
    return 0;
}
```

