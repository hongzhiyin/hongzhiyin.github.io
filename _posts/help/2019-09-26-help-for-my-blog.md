---
layout:        post
title:         "博客搭建 · 帮助手册"
subtitle:      "Help for building my blog"
author:        "hongzhiyin"
header-style:  text
catalog:       true
tags:
  - 休伯利安
  - 帮助
---

## 致谢

感谢 [Hux](https://huangxuan.me) 设计的博客主题。

## 撰写博文

### 文件名

时间 + 标题，空格用 `-` 替代。

```
2019-09-26-help-for-my-blog.md
```



### YAML 头信息

参考文档：http://jekyllcn.com/docs/frontmatter

```c
layout:        post                 // 使用 _layouts 目录下的模板文件 post
title:         "..."                // 标题
subtitle:      "..."                // 副标题
date:          2019-09-26 12:00:00  // 文章显示时间，可覆盖文件名的时间
author:        "..."                // 作者
hidden:        true                 // 在首页隐藏
header-img:    "img/example.jpg"    // 页眉图片，保存在 img 目录下
header-mask:   0.3                  // 图片遮罩不透明度 0 ~ 1
header-bg-css: "linear-gradient(to right, #24b94a, #38ef7d);"
                                    // 页眉使用从左到右线性梯度的纯色图片
header-style:  text                 // 使用 text 风格的页眉，即无图片
mathjax:       true                 // 显示数学公式
catalog:       true                 // 文章右侧显示目录
tags:                               // 标签
  - tag1
  - tag2
```

