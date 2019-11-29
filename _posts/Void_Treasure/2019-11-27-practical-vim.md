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

`x` 命令删除光标下的字符，

`.` 命令重复上次修改。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two |
| `x`      | <span style="color:white;background:black;">i</span>ne one<br />Line two |
| `.`      | <span style="color:white;background:black;">n</span>e one<br />Line two |
| `..`     | <span style="color:white;background:black;">&ensp;</span>one<br />Line two |



**例 2**

`dd` 命令删除一整行。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two<br />Line three |
| `dd`     | <span style="color:white;background:black;">L</span>ine two<br />Line three |
| `.`      | <span style="color:white;background:black;">L</span>ine three |



**例 3**

`>G` 命令增加从当前行到文档末尾处的缩进层级，

`j` 命令使光标下移一行。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | Line one<br /><span style="color:white;background:black;">L</span>ine two<br />Line three |
| `>G`     | Line one<br />&emsp;<span style="color:white;background:black;">L</span>ine two<br />&emsp;Line three |
| `j`      | Line one<br />&emsp;Line two<br />&emsp;<span style="color:white;background:black;">L</span>ine three |
| `.`      | Line one<br />&emsp;Line two<br />&emsp;&emsp;<span style="color:white;background:black;">L</span>ine three |



## 技巧 2 不要自我重复

**例 1**

在每行的结尾添加一个分号。

`A` 命令将光标移到行尾，进入插入模式。

| 按键操作  | 缓冲区内容                                                   |
| --------- | ------------------------------------------------------------ |
| {start}   | <span style="color:white;background:black;">v</span>ar foo = 1<br />var bar = 'a'<br />var foobar = foo + bar |
| `A;<Esc>` | var foo = 1<span style="color:white;background:black;">;</span><br />var bar = 'a'<br />var foobar = foo + bar |
| `j`       | var foo = 1;<br />var bar = '<span style="color:white;background:black;">a</span>'<br />var foobar = foo + bar |
| `.`       | var foo = 1;<br />var bar = 'a'<span style="color:white;background:black;">;</span><br />var foobar = foo + bar |
| `j.`      | var foo = 1;<br />var bar = 'a';<br />var foobar = foo + bar<span style="color:white;background:black;">;</span> |



## 技巧 3 以退为进

**例 1**

在 `+` 字符前后各添加一个空格。

`f{char}` 命令查找行内下一处指定字符出现的位置，将光标移到该字符位置，

`s` 命令删除光标下的字符，进入插入模式，

`;` 命令重复查找上次 `f` 命令所查找的字符。

| 按键操作    | 缓冲区内容                                                   |
| ----------- | ------------------------------------------------------------ |
| {start}     | <span style="color:white;background:black;">v</span>ar foo = "method("+argument1+","+argument2+")"; |
| `f+`        | var foo = "method("<span style="color:white;background:black;">+</span>argument1+","+argument2+")"; |
| `s + <Esc>` | var foo = "method(" +<span style="color:white;background:black;">&ensp;</span>argument1+","+argument2+")"; |
| `;`         | var foo = "method(" + argument1<span style="color:white;background:black;">+</span>","+argument2+")"; |
| `.`         | var foo = "method(" + argument1 +<span style="color:white;background:black;">&ensp;</span>","+argument2+")"; |
| `;.`        | var foo = "method(" + argument1 + "," +<span style="color:white;background:black;">&ensp;</span>argument2+")"; |
| `;.`        | var foo = "method(" + argument1 + "," + argument2 +<span style="color:white;background:black;">&ensp;</span>")"; |



## 技巧 4 执行、重复、回退

**可重复的操作及如何回退**

`t{char}` 命令查找行内下一处指定字符出现的位置，将光标移到该字符位置前。

| 目的                     | 操作                    | 重复 | 回退 |
| ------------------------ | ----------------------- | ---- | ---- |
| 做出一个修改             | {edit}                  | `.`  | `u`  |
| 在行内查找下一个指定字符 | `f{char}` / `t{char}`   | `;`  | `,`  |
| 在行内查找上一个指定字符 | `F{char}` / `T{char}`   | `;`  | `,`  |
| 在文档中查找下一处匹配项 | `/pattern<CR>`          | `n`  | `N`  |
| 在文档中查找上一处匹配项 | `?pattern<CR>`          | `n`  | `N`  |
| 执行替换                 | `:s/target/replacement` | `&`  | `u`  |
| 执行一系列修改           | `qx{changes}q`          | `@x` | `u`  |



## 技巧 5 查找并手动替换

