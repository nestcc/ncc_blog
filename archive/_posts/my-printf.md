---
title: 实现c语言中的printf()
date: 2020-08-11 09:19:36
tags:
- C++
categories:
- course
---



# 封装

我们将整个函数封装进一个头文件中，命名为"customprt.h"。在这个头文件中我们定义一个函数，命名为"custom_prt"。

```c++
#pragma once
#include <cstdio>
#include <cstdarg>

int custom_prt(const char* frm, ...) {
    ...
}
```





# 首先实现打印不带占位符的字符串



```c++
int custom_prt(const char* frm) {
	int count = 0;
#define PUTC(a) putchar(a), count += 1
	for (int i = 0; frm[i]; i++) {
		PUTC(frm[i]);
	}
#undef PUTC
}
```

我们可以使用putc()打印每一个字符。并把它封装进一个宏中。



# 接下来我们处理存在占位符的情况

## 发现"%..."时的处理

```c++
if (frm[i] == '%') {							// 处理"%d","%s","%%"
    i += 1;										// 跳过"%.."后的'd','s','%'
    ...
    
}
else {											// 否则直接打印
    PUTC(frm[i]);
    count += 1;
}
```



## 处理"%%"，打印一个'%'

```c++
if (frm[i] == '%') {
    PUTC(frm[i]);
    count += 1;
} 
```



## 处理"%d"

```c++
else if (frm[i] == 'd') {
    long long x = va_arg(arg, int);			// 处理INT32_MAX，INT32_MIN，需使用long long
    if (x < 0) {							// 负数情况处理
        PUTC('-');
        x = - x;
        count += 1;
    }
    char tmp[20];							// 以字符串形式存储数字
    int digit = 0;							// 总位数

    do {
        tmp[digit] = x % 10 + '0';			// 将每一位存入tmp中，此时为倒序，如123存入后为['3','2','1']
        digit += 1;							// 位数 + 1
        x /= 10;
    } while (x);

    count += digit;

    while (digit--) {						// 倒序打印
        PUTC(tmp[digit]);
    }
}
```





## 处理"%s"

```c++
else if (frm[i] == 's') {
    const char* chr = va_arg(arg, const char*);
    for (int i = 0; chr[i]; i += 1) {
        PUTC(chr[i]);
        count += 1;
    }
}
```



## 完整程序

```c++
#pragma once
#include <cstdio>
#include <cstdarg>

int custom_prt(const char* frm, ...) {
	int count = 0;
	va_list arg;
	va_start(arg, frm);
#define PUTC(a) putchar(a), count += 1
	for (int i = 0; frm[i]; i++) {
		if (frm[i] == '%') {							// 处理"%d","%s","%%"
			i += 1;
			if (frm[i] == '%') {
				PUTC(frm[i]);
			} 
			else if (frm[i] == 'd') {
				long long x = va_arg(arg, int);			// 处理INT32_MAX，INT32_MIN，需使用long long
				if (x < 0) {							// 负数情况处理
					PUTC('-');
					x = - x;
				}
				char tmp[20];							// 以字符串形式存储数字
				int digit = 0;							// 总位数

				do {
					tmp[digit] = x % 10 + '0';			// 将每一位存入tmp中，此时为倒序，如123存入后为['3','2','1']
					digit += 1;							// 位数 + 1
					x /= 10;
				} while (x);

				while (digit--) {						// 倒序打印
					PUTC(tmp[digit]);
				}
			}
			else if (frm[i] == 's') {
				const char* chr = va_arg(arg, const char*);
				for (int i = 0; chr[i]; i += 1) {
					PUTC(chr[i]);
				}
			}
		}
		else {											// 否则直接打印
			PUTC(frm[i]);
		}
	}
#undef PUTC
	return count;
}
```





## 测试

运行

```c++
#include "customprt.h"
#include <cinttypes>


int main() {
	custom_prt("%d\n", INT32_MAX);
	printf("%d\n", INT32_MAX);

	custom_prt("%d\n", -5);
	printf("%d\n", -5);

	custom_prt("%d\n", INT32_MIN);
	printf("%d\n", INT32_MIN);

	int n = -500;

	printf("has %d\n", custom_prt("%d%s\n", n, "aa"));
	printf("has %d\n", printf("%d%s\n", n, "aa"));

	return 0;
}
```



结果

```
2147483647
2147483647
-5
-5
-2147483648
-2147483648
-500aa
has 7
-500aa
has 7
```

