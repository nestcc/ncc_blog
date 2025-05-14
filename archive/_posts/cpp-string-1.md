---
title: C++字符串
date: 2020-08-18 09:29:48
tags:
    - C++
    - String
categories:
    - Dev
---
# c++字符串

头文件：string.h

| func                   | discription      |
| ---------------------- | ---------------- |
| strlen(str)            |                  |
| strcmp(str1, str2)     |                  |
| strcpy(dest, src)      | src拷贝到dest中  |
| strncmp(str1, str2, n) | 安全的字符串比较 |
| strncpy(str1, str2, n) | 安全的字符串拷贝 |
| memcpy(str1, str2, n)  | 内存拷贝         |
| memcmp(str1, str2, n)  | 内存比较         |
| memset(str1, c, n)     | 内存设置         |

内存拷贝不需要两个参数类型相同，他会直接复制内存中的数据，其运行速度最快。

**尽量不要使用不安全的拷贝和比较，因为c的字符串拷贝机制是以'\0'为结束的，那么如果src比dest长，那么拷贝将会越界。**
