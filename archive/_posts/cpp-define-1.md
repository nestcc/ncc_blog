---
title: C++宏定义
date: 2020-08-15 14:05:46
tags:
    - C++
categories:
    - Dev
---
# 如何使用宏

所有带“#”的代码都是宏，例如**#include \<cstdio\>**就是一个宏。

我们有如下几种定义宏的方法。

## 定义符号常量

```c++
#define PI 3.1415926
#define MAX_N 10000
```

## 定义表达式

```c++
#define MAX(a, b) (a) > (b) ? (a) : (b)
#define S(a, b) a * b
```

## 定义代码段

```c++
#define P(a) { \
	printf("%d\n", a); \
}
```

## 变参宏

```c++
#define log(frm, args...) {\
	printf("[%s : %d] %s = ", __func__, __LINE__, #args);\
	printf(frm, ##args);\
	print("\n");\
}
```

## 注意

**宏仅作代码替换操作，不做逻辑处理**

例如：

```c++
#define S(a, b) a * b
```

如果我们调用：

```c++
S(2 + 3, 4 + 5)
```

代码会被替换成：

```c++
2 + 3 * 4 + 5
```

## 例

```c++
#define DEBUG
#ifdef DEBUG
#define log(frm, args...) {\
	printf("[%s : %d] %s = ", __func__, __LINE__, #args);\
	printf(frm, ##args);\
	print("\n");\
}
#else
#define log(frm, args...)
#endif
```

这样我们就定义了一个log宏，可以在debug时期打印log，如：

```c++
int main() {
    int a = 123;
    log("%d", a);
    log("Hello world");
    return 0;
}
```

如果我们进行完debug，不想再要打印信息了，只需要将"#define DEBUG"注释掉即可。这样log宏就会展开成空。

我们可以使用命令添加宏，如：

```bash
g++ -DDEBUG xxx.cpp	// 用于debug，有log信息
```

```bash
g++ xxx.cpp // 用于执行，没有log信息
```

# 预定义的宏

| 说明                     | 函数                    |
| ------------------------ | ----------------------- |
| \_\_DATE\_\_             | 日期：M mm dd yyyy      |
| \_\_TIME\_\_             | 时间：hh:mm:ss          |
| \_\_LINE\_\_             | 行号                    |
| \_\_FILE\_\_             | 文件名                  |
| \_\_func\_\_             | 函数名/非标准           |
| \_\_FUNC\_\_             | 函数名/非标准           |
| \_\_PRETTY\_FUNCTION\_\_ | 更详细的函数信息/非标准 |

# 条件式编译

| 函数             | 说明                 |
| ---------------- | -------------------- |
| #ifdef DEBUG     | 是否定义了DEBUG      |
| #idndef DEBUG    | 是否没有定义DEBUG    |
| #if MAX_N == 5   | 宏MAX_N是否等于5     |
| #elif MAX_N == 4 | 否则宏MAX_N是否等于4 |
| #else            |                      |
| #endif           |                      |

# 预处理命令

![cppdefineproc](C:\myProjects\nccBlog\source\cppdefine\cppdefineproc.png)

宏的处理发生在预处理阶段，在编译阶段之前。

c++编译器在预处理阶段会把所有的宏替换为代码，例如将"stdio.h"文件内容拷贝进来。

由于宏的处理发生在预编译阶段，宏的执行速度也是最快的，其处理速度远快于函数。
