---
layout:        post
title:         "扫描线"
subtitle:      "Scanning Line"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 扫描线
  - 线段树
---

### 矩形面积并

**题意** 

[洛谷 P5490](https://www.luogu.org/problem/P5490) 

输入 $n$ 个矩形，以对角线坐标表示，求这 $n$ 个矩形的面积并。

**方法** 

此处采用从下到上的扫描线

因此需对横坐标离散化，使其能使用线段树

对于每个矩形，分为上下两条线段

将所有线段按 $y$ 坐标从小到大排序

扫描线从下往上扫描，实际上就是从下往上遍历每一条线段

矩形的下边界在扫描线对应区间上 $+1$ ，上边界反之

每次累加：当前扫描线上被覆盖的长度 乘以 到下一条线段的距离

即扫描线在两条线段之间扫过的面积

最终即为所有矩形的面积并



**注意**

1. 每个矩形得到左右两个端点，即线段树右边界最大为 $2n$ ，也可以是离散化数组大小 $sz(dis)$
2. 线段树区间更新时，区间左闭右开的原因：
   - 可以把区间每个点看作点到右边下一个点的这段距离
   
     因此， $n$ 个点对应 $n-1$ 段距离
   
     所以在更新覆盖时，不需要考虑最右边的点，因为该点对应的那段距离没有被覆盖
   
     但在计算整个被覆盖区间的长度时，需要用到最右边点的离散化前的值
   
     即区间 $[l, r]$ 对应长度为 $dis[r-1] - dis[l-1]$ ，减 $1$ 是因为离散值从 $1$ 开始
   
     因为在传入线段树时是传入 $r-1$ ，所以还原时用的是 $dis[r+1-1] = dis[r]$ 
3. 线段树中，更新完对应结点时，向上统计答案时
   1. 当有标记时，记区间被完全覆盖
   2. 无标记，区间只有 $1$ 个点，记覆盖长度为 $0$ ，主要是为了防止越界
   3. 无标记，则长度由左右儿子传递而来
4. 此合并方式可行的原因
   1. 只需要根节点的答案
   2. 标记只需要看是否为 $0$ 
   3. 因此，子节点不需要更新，即不需要 $pushdown$ 
   
      因为向上合并过程中只要父节点有标记，即认为区间完全覆盖
   
      因此根节点一定能收到这个信息



```c++
/* <<head>> */
#define y1 y100
const int N = (int)1e5+7;

// ------- 变量 ------- //

struct Line {
    int l, r, y, f;
    Line () {} Line (int l, int r, int y, int f) : l(l), r(r), y(y), f(f) {} 
    bool operator < (const Line &rhs) const { return y < rhs.y; }
} a[N<<1];

int n, nn, x1[N], x2[N], y1[N], y2[N];
vi dis;

// ------- 函数 ------- //

#define ls rt << 1
#define rs rt << 1 | 1
#define lson l, m, ls
#define rson m + 1, r, rs
struct { int cnt, len; } t[N<<1 << 2];                   // 下标范围为 2n
void upd(int L, int R, int val, int l, int r, int rt) {
    if (L <= l && r <= R) {
        t[rt].cnt += val;
        if (t[rt].cnt) t[rt].len = dis[r] - dis[l-1];    // 区间完全覆盖
        else if (l == r) t[rt].len = 0;                  // 防止越界
        else t[rt].len = t[ls].len + t[rs].len;
        return ;
    }
    int m = (l + r) >> 1;
    if (L <= m) upd(L, R, val, lson);
    if (m < R) upd(L, R, val, rson);
    if (t[rt].cnt) t[rt].len = dis[r] - dis[l-1];
    else t[rt].len = t[ls].len + t[rs].len;
}

void Init() {
    scanf("%d", &n);
    
    // 离散化
    dis.clear();
    rep(i, 0, n) {
        scanf("%d%d%d%d", x1+i, y1+i, x2+i, y2+i);
        if (x1[i] > x2[i]) swap(x1[i], x2[i]);
        if (y1[i] > y2[i]) swap(y1[i], y2[i]);
        dis.pb(x1[i]); dis.pb(x2[i]);
    }
    sort(all(dis));
    dis.erase(unique(all(dis)), dis.end());
    
    // 矩形分为上下两条线段
    rep(i, 0, n) {
        int l = lower_bound(all(dis), x1[i]) - dis.begin() + 1;
        int r = lower_bound(all(dis), x2[i]) - dis.begin() + 1;
        a[i<<1] = Line(l, r, y1[i], 1);
        a[i<<1|1] = Line(l, r, y2[i], -1);
    }
    nn = n << 1;
    sort(a, a + nn);
}

int Solve() {
    memset(t, 0, sizeof(t[0]) * (nn<<2));            // 初始化线段树
    ll ans = 0;
    rep(i, 0, nn-1) {                                // 最后一条线段不需要遍历
        upd(a[i].l, a[i].r-1, a[i].f, 1, nn, 1);     // 更新扫描线上被覆盖长度
        ans += 1ll * t[1].len * (a[i+1].y - a[i].y); // 到下一条线段时扫过的面积
    }
    return printf("%lld\n", ans);
}
```

