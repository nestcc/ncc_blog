---
title: 欧拉计划5
date: 2020-08-08 09:32:08
tags:
    - C++
    - Euler project
categories:
    - Algorithm
---



## **Smallest multiple**

2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

What is the smallest positive number that is evenly divisible by all of the numbers from 1 to 20?



---



问题可等同于求1-20的最小公倍数。

方法：lcm(1, 2) => lcm(lcm(1, 2), 3) => lcm(lcm(...), 20)

其中，lcm(a, b) = (a * b) / gcd(a, b)



```c++
#include <cstdio>
#define max_n 20

int gcd(int a, int b) {
    return (b ? gcd(b, a % b) : a);
}

int main() {
    int ans = 1;
    
    for (int i = 1; i <= max_n; i += 1) {
        ans *= i / gcd(ans, i);
    }
    
    printf("%d\n", ans);
    return 0;
}

```

