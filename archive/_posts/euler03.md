---
title: 欧拉计划3
date: 2020-08-06 09:11:45
tags:
- C++
- Euler project
categories:
- course
---



# **Largest prime factor**

The prime factors of 13195 are 5, 7, 13 and 29.

What is the largest prime factor of the number 600851475143 ?



```c++
#include <stdio.h>
#define max_n 600851475143LL

int main() {
    int ans = 0, i = 2;
    long long n = max_n;
    while (i * i <= n) {
        if (n % i == 0) ans = i;
        while (n % i == 0) n /= i; // 除掉所有为i的质因子，本行之后n与i互质
        i++;
    }
    if (n != 1) ans = n;
    printf("%d\n", ans);
    return 0;
}
```

