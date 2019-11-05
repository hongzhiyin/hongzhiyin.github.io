---
layout:        post
title:         "线段树"
subtitle:      "Segment Tree"
author:        "hongzhiyin"
header-style:  text
mathjax:       true
catalog:       true
tags:
  - 虚空万藏
  - 线段树
---

## 定义

// 形象





## 解题过程

1. 前提：区间查询的结果满足区间可加性。

2. 列出关于区间修改的表达式

   1. 列出关于单点修改的表达式
      $$
      x_i = F(x_i)
      $$

   2. 列出关于区间修改的表达式
      $$
      sum(l, r) = F(\ sum(l, r)\ )
      $$

   3. 列出重复在同一区间上修改 $k$ 次的表达式
      $$
      sum(l, r) = F(\ sum(l, r),\ k\ )
      $$

   4. 包含有 $k$ 的部分即为延迟标记需要维护的部分
      $$
      sum(l, r) = F(\ lazy(k),\ sum(l, r)\ )
      $$



## 例题

