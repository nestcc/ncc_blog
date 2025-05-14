---
title: 欧拉计划15
date: 2020-08-06 09:06:10
tags:
    - C++
    - Euler project
categories:
    - Algorithm
---



# **Lattice paths**

Starting in the top left corner of a 2×2 grid, and only being able to move to the right and down, there are exactly 6 routes to the bottom right corner.

How many such routes are there through a 20×20 grid?



#### 排列组合解决



```c++
#include <stdio.h>

int main() {
    long long m = 40, n = 20, ans = 1;
    while (m != 20 || n) {
        if (m != 20) ans *= m--;
        if (n && ans % n == 0) ans /= n--;
    }
    printf("%lld\n", ans);
    return 0;
}
```

