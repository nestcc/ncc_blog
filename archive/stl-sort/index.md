---
title: stl-sort
date: 2020-11-03 13:32:00
categories:
- c++
---





# STL SORT库的使用

对于任意一个结构体列表：

```c++
struct node {
  int x;
  ...
};

node ns[100];
```

我们可以自定义一个比较函数：

```c++
bool cmp(node &n1, node &n2) {
  
  return n1.x > n2.x;
}
```

然后将其作为参数传人sort函数中：

```c++
std::sort(ns, ns + 100, cmp);
```

若cmp的返回值为1，则参与比较的两个结构体对象顺序正确，不需要调整顺序，反之则需要调整顺序。