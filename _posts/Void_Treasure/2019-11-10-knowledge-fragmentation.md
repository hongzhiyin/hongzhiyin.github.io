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



---



**int main(int argc, char *argv[])**

argc 是 argument count 的缩写，表示传入 main 函数的参数个数。

argv 是 argument vector 的缩写，表示传入 main 函数的参数序列或指针，并且第一个参数argv[0] 一定是程序的名称，并且包含了程序所在的完整路径，所以确切的说需要我们输入的 main 函数的参数个数应该是 argc - 1 个。

举例：

设有一文件 `a.cpp` ，其内容如下：

```c++
#include <iostream>

using namespace std;

int main(int argc, char *argv[]) {
    for (int i = 0; i < argc; i++) {
        cout << "argument[" << i << "] is : " << argv[i] << endl;
    }
    return 0;
}
```

将其编译为可执行文件 `a.exe` ，其目录为 `C:\example\a.exe` ，

在命令行中按如下方式执行：

```
C:\example\a.exe arg1 arg2
```

输出结果为：

```
argument[0] is : C:\example\a.exe
argument[1] is : arg1
argument[2] is : arg2
```

