---
layout:        post
title:         "知识碎片"
subtitle:      "Knowledge Fragmentation"
author:        "hongzhiyin"
header-style:  text
catalog:       true
tags:
  - 虚空万藏
---

**hosts**

定义：

Hosts 是一个没有扩展名的系统文件，可以用记事本等工具打开，地址在： `C:\Windows\System32\drivers\etc\hosts`

作用：

将网址域名与其对应的 IP 地址建立一个关联数据库，

当输入网址域名时，系统首先自动从 Hosts 文件中寻找对应的 IP 地址，

一旦找到，系统会立即打开对应网页，

如果没有找到，则系统再将网址提交 DNS 域名解析服务器进行 IP 地址的解析。

注意：

Hosts 文件配置的映射是静态的，如果网络上的计算机更改了 IP 地址请及时更新，否则将不能访问。