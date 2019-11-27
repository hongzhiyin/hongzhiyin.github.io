---
layout:        post
title:         "《Vim 实用技巧》"
subtitle:      "Practical Vim"
author:        "hongzhiyin"
header-style:  text
catalog:       true
tags:
  - 虚空万藏
---

## 技巧 1 结识 . 命令

**例 1**

`x` 命令删除光标下的字符， `.` 命令重复上次修改，即删除光标下的字符。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two |
| `x`      | <span style="color:white;background:black;">i</span>ne one<br />Line two |
| `.`      | <span style="color:white;background:black;">n</span>e one<br />Line two |
| `..`     | <span style="color:white;background:black;">&ensp;</span>one<br />Line two |



**例 2**

`dd` 命令删除一整行， `.` 命令重复上次修改，即删除一整行。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two<br />Line three |
| `dd`     | <span style="color:white;background:black;">L</span>ine two<br />Line three |
| `.`      | <span style="color:white;background:black;">L</span>ine three |