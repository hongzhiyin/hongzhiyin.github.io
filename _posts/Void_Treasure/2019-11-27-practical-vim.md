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

**重复删除光标下的字符**

`x` 删除光标下的字符。

`.` 重复上次修改。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two |
| `x`      | <span style="color:white;background:black;">i</span>ne one<br />Line two |
| `.`      | <span style="color:white;background:black;">n</span>e one<br />Line two |
| `..`     | <span style="color:white;background:black;">&ensp;</span>one<br />Line two |



**重复删除一整行**

`dd` 删除一整行。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | <span style="color:white;background:black;">L</span>ine one<br />Line two<br />Line three |
| `dd`     | <span style="color:white;background:black;">L</span>ine two<br />Line three |
| `.`      | <span style="color:white;background:black;">L</span>ine three |



**重复增加从当前行到文档末尾处的缩进层级**

`>G` 增加从当前行到文档末尾处的缩进层级。

`j` 使光标下移一行。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | Line one<br /><span style="color:white;background:black;">L</span>ine two<br />Line three |
| `>G`     | Line one<br />&emsp;<span style="color:white;background:black;">L</span>ine two<br />&emsp;Line three |
| `j`      | Line one<br />&emsp;Line two<br />&emsp;<span style="color:white;background:black;">L</span>ine three |
| `.`      | Line one<br />&emsp;Line two<br />&emsp;&emsp;<span style="color:white;background:black;">L</span>ine three |



## 技巧 2 不要自我重复

**在每行的结尾添加一个分号**

`A` 将光标移到行尾，进入插入模式。

| 按键操作  | 缓冲区内容                                                   |
| --------- | ------------------------------------------------------------ |
| {start}   | <span style="color:white;background:black;">v</span>ar foo = 1<br />var bar = 'a'<br />var foobar = foo + bar |
| `A;<Esc>` | var foo = 1<span style="color:white;background:black;">;</span><br />var bar = 'a'<br />var foobar = foo + bar |
| `j`       | var foo = 1;<br />var bar = '<span style="color:white;background:black;">a</span>'<br />var foobar = foo + bar |
| `.`       | var foo = 1;<br />var bar = 'a'<span style="color:white;background:black;">;</span><br />var foobar = foo + bar |
| `j.`      | var foo = 1;<br />var bar = 'a';<br />var foobar = foo + bar<span style="color:white;background:black;">;</span> |



## 技巧 3 以退为进

**在 '+' 字符前后各添加一个空格**

`f{char}` 查找行内下一处指定字符出现的位置，将光标移到该字符位置。

`s` 删除光标下的字符，进入插入模式。

`;` 重复查找上次 `f` 命令所查找的字符。

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

`t{char}` 查找行内下一处指定字符出现的位置，将光标移到该字符位置前。

`pattern` 、 `target` 、 `replacement` 皆替换为实际查找或替换的词

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

**替换部分单词**

`:se hls` 高亮当前匹配项。

`:se nohls` 取消高亮当前匹配项。

`*` 查找当前光标下的单词，并跳到下一个匹配项上。

`cw` 删除光标下的单词，进入插入模式。

| 按键操作      | 缓冲区内容                                                   |
| ------------- | ------------------------------------------------------------ |
| {start}       | ...We' re waiting for content before the site can go live...<br />...If you are <span style="color:white;background:black;">c</span>ontent with this, let's go ahead with it...<br />...We'll launch as soon as we have the content... |
| `*`           | ...We' re waiting for <span style="color:black;background:grey;">content</span> before the site can go live...<br />...If you are <span style="color:black;background:grey;">content</span> with this, let's go ahead with it...<br />...We'll launch as soon as we have the <span style="color:white;background:black;">c</span><span style="color:black;background:grey;">ontent</span>... |
| `cwcopy<Esc>` | ...We' re waiting for <span style="color:black;background:grey;">content</span> before the site can go live...<br />...If you are <span style="color:black;background:grey;">content</span> with this, let's go ahead with it...<br />...We'll launch as soon as we have the cop<span style="color:white;background:black;">y</span>... |
| `n`           | ...We' re waiting for <span style="color:white;background:black;">c</span><span style="color:black;background:grey;">ontent</span> before the site can go live...<br />...If you are <span style="color:black;background:grey;">content</span> with this, let's go ahead with it...<br />...We'll launch as soon as we have the copy... |
| `.`           | ...We' re waiting for cop<span style="color:white;background:black;">y</span> before the site can go live...<br />...If you are <span style="color:black;background:grey;">content</span> with this, let's go ahead with it...<br />...We'll launch as soon as we have the copy... |



## 技巧 6 结识 . 范式

理想模式：用一键移动，另一键执行。



## 技巧 7 停顿时请移开画笔

想一下除了画画外，画家还要做哪些事情。他们要研究主题，调整光线，把颜料混合成新的色
彩。而且，在把颜料往画布上画时，谁说他们必须要用画笔？画家也许会换用刻刀来实现不同的质地，或是用棉签来对已经画好的地方进行润色。

画家在休息时不会把画笔放在画布上。对 Vim 而言也是这样，普通模式就是 Vim 的自然放松状态，其名字已经寓示了这一点。

就像画家只花一小部分时间涂色一样，程序员也只花一小部分时间编写代码。绝大多数时间用来思考、阅读、以及在代码中穿梭浏览。而且，当确实需要做修改时，谁说一定要切换到插入模式才行？我们可以重新调整已有代码的格式，复制它们，移动其位置，或是删除它们。在普通模式中，我们有众多的工具可以利用。



## 技巧 8 把撤销单元切成块

一般来讲，如果你停顿的时间长到足以问 “ 我应该退出插入模式吗？” 这个问题，那么就退出吧。

如果在插入模式中使用了 `<Up>` 、 `<Down>` 、 `<Left>` 或 `<Right>` 这些光标键，将会产生一个新的撤销块。 你可以把这想象为先切换回普通模式，然后用 `h` 、 `j` 、 `k` 或 `l` 命令对光标进行了移动，唯一区别是我们并没有退出插入模式。

**这也会对 `.` 命令的操作产生影响。**



## 技巧 9 构造可重复的修改

**删除一个单词**

`daw` 删除一个单词， `aw` 是一个文本对象。可解读为 " delete a word " 。

| 按键操作 | 缓冲区内容                                                   |
| -------- | ------------------------------------------------------------ |
| {start}  | The end is nig<span style="color:white;background:black;">h</span> |
| `daw`    | The end i<span style="color:white;background:black;">s</span> |

要想充分利用 `.` 命令，事先常常需要进行一番周详的考虑。如果你发现自己要在几个地方做同样的小修改，就可以尝试构造你的修改，让它们能够被 `.` 命令重复执行。

要识别出这类机会需要进行一定的实践，不过一旦你养成了使修改可重复的习惯，那么你就会从 Vim 这里得到 “ 奖赏 ” 。

有时，我并没有看到用 `.` 命令的机会，然而在做完一次修改后，我发现要做另一次同样的操作，这时候，我脑海里会浮现出 `.` 命令，而它也已经准备好为我效力了。每当遇到这种情况时，我都会开心地笑起来。



## 技巧 10 用次数做简单的算术运算

`<C-a>` 把当前光标之上或之后的数值加 1 。

`<C-x>` 把当前光标之上或之后的数值减 1 。

`yy` 复制一整行。

`p` 粘贴。

`w` 移到下一个单词开头，标点符号看作下一个单词。

`W` 移到下一个单词开头，标点符号看作当前单词的内容。

`cW` 修改当前光标所在的单词。

| 按键操作       | 缓冲区内容                                                   |
| -------------- | ------------------------------------------------------------ |
| {start}        | .blog, .news { background-image: url(/sprite.png); }<br /><span style="color:white;background:black;">.</span>blog { background-position: 0px 0px } |
| `yyp`          | .blog, .news { background-image: url(/sprite.png); }<br />.blog { background-position: 0px 0px }<br /><span style="color:white;background:black;">.</span>blog { background-position: 0px 0px } |
| `cW.news<Esc>` | .blog, .news { background-image: url(/sprite.png); }<br />.blog { background-position: 0px 0px }<br />.new<span style="color:white;background:black;">s</span> { background-position: 0px 0px } |
| `180<C-x>`     | .blog, .news { background-image: url(/sprite.png); }<br />.blog { background-position: 0px 0px }<br />.news { background-position: -18<span style="color:white;background:black;">0</span>px 0px } |



## 技巧 11 能够重复，就别用次数

执行、重复、回退。

只在必要时使用次数。



## 技巧 12 双剑合璧，天下无敌

$$
\Large 操作符 + 动作命令 = 操作
$$

**操作符**

| 命令 | 用途       |
| ---- | ---------- |
| `c`  | 修改       |
| `d`  | 删除       |
| `y`  | 复制       |
| `g~` | 反转大小写 |
| `gu` | 转换为小写 |
| `gU` | 转换为大写 |
| `>`  | 增加缩进   |
| `<`  | 减小缩进   |
| `=`  | 自动缩进   |

当一个操作符命令被连续调用两次时，它会作用于当前行。

**动作命令**

`l` 一个字符。

`w` 从光标开始往后一个单词。

`aw` 光标当前所属的一个完整单词。

`ap` 一整个段落。

`G` 从光标位置到文件结尾的所有内容。

**自动缩进整个文件**

`gg=G`

`gg` 跳到文件开头， `=G` 自动缩进从光标位置到文件结尾的所有内容。