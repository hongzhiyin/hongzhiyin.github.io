---
layout:        post
title:         "《Vim 实用技巧》"
subtitle:      "Practical Vim"
author:        "hongzhiyin"
header-style:  text
catalog:       true
tags:
  - 虚空万藏
  - Vim
---

## 技巧 1 结识 . 命令

**例 1**

`x` 命令删除光标下的字符， `.` 命令重复上次修改。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two |
| `x`      | <span style="color:white;background:black;">i</span>ne one<br />Line two |
| `.`      | <span style="color:white;background:black;">n</span>e one<br />Line two |
| `..`     | <span style="color:white;background:black;">&ensp;</span>one<br />Line two |



**例 2**

`dd` 命令删除一整行， `.` 命令重复上次修改。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two<br />Line three |
| `dd`     | <span style="color:white;background:black;">L</span>ine two<br />Line three |
| `.`      | <span style="color:white;background:black;">L</span>ine three |



**例 3**

`>G` 命令增加从当前行到文档末尾处的缩进层级， `.` 命令重复上次修改。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | Line one<br /><span style="color:white;background:black;">L</span>ine two<br />Line three |
| `>G`     | Line one<br />&emsp;<span style="color:white;background:black;">L</span>ine two<br />&emsp;Line three |
| `j`      | Line one<br />&emsp;Line two<br />&emsp;<span style="color:white;background:black;">L</span>ine three |
| `.`      | Line one<br />&emsp;Line two<br />&emsp;&emsp;<span style="color:white;background:black;">L</span>ine three |



## 技巧 2 不要自我重复

**例 1**

| 按键操作  | 缓冲区内容                                                   |
| --------- | ------------------------------------------------------------ |
| {start}   | <span style="color:white;background:black;">v</span>ar foo = 1<br />var bar = 'a'<br />var foobar = foo + bar |
| `A;<Esc>` | var foo = 1<span style="color:white;background:black;">;</span><br />var bar = 'a'<br />var foobar = foo + bar |
| `j`       | var foo = 1;<br />var bar = '<span style="color:white;background:black;">a</span>'<br />var foobar = foo + bar |
| `.`       | var foo = 1;<br />var bar = 'a'<span style="color:white;background:black;">;</span><br />var foobar = foo + bar |
| `j.`      | var foo = 1;<br />var bar = 'a';<br />var foobar = foo + bar<span style="color:white;background:black;">;</span> |



## 技巧 3 以退为进

