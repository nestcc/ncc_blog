---
title: C++STL标准库实现2——Vector 实现
date: 2020-08-03 10:43:01
tags:
    - Development
    - C++
categories:
    - [Dev, c++]
---







# vector 介绍



vector 是 C++ 标准模板库中的部分内容，它是一个多功能的，能够操作多种数据结构和算法的模板类和函数库。vector 是向量类型，它可以容纳多种类型的数据，所以称之为容器。



# vector 类基础



``` cpp
private:
    T *start_;						// 表示使用空间的头
    T *finish_;						// 表示使用空间的末尾
    T *endofStorage_;				// 表示可用空间的末尾
    typedef Alloc dataAllocator;	// 表示分配空间
```

