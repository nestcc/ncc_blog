---
title: 欧拉计划1 & 6
date: 2020-07-28 21:10:56
tags:
- C++
- Euler project
categories:
- course
---



## **01. Multiples of 3 and 5**

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.



>    暴力解 

``` c++
#include <stdio.h>

int is_val(int n) {
    return n % 3 == 0 || n % 5 == 0;
}

int main() {
    int res = 0;
    for (int i = 1; i < 1000; i++) {
        if (is_val(i)) res += i;
    }
    
    printf("%d\n", res);
    return 0;
}
```

*   时间复杂度O(n)



>   等差

``` c++
#include <stdio.h>

int main() {
    int sum_3 = (3 + 999) * 333 / 2;
    int sum_5 = (5 + 995) * 199 / 2;
    int sum_15 = (15 + 999 / 15 * 15) * (999 / 15) / 2;
    
    printf("%d\n", sum_3 + sum_5 - sum_15);
    return 0;
}
```

*   时间复杂度O(1)



## **06. Sum square difference**

The sum of the squares of the first ten natural numbers is,

12 + 22 + ... + 102 = 385

The square of the sum of the first ten natural numbers is,

(1 + 2 + ... + 10)2 = 552 = 3025

Hence the difference between the sum of the squares of the first ten natural numbers and the square of the sum is 3025 − 385 = 2640.

Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.



>   老规矩，暴力解，O(n)

``` c++
#include <stdio.h>

int main() {
    
    int sum_of_squ = 0; squ_of_sum = 0;
    
    for (int i = 0; i <= 100; i += 1) {
        sum_of_sqq += i * i;
        squ_of_sum += i;
    }
    
    printf("%d\n", squ_of_sum * squ_of_sum - sum_of_squ);
    return 0;
}
```



>   去for循环，公式自行百度

``` c++
#include <stdio.h>
#define n 100

int main() {
    int sum2 = 5050;
    int sum1 = (2 * n * n * n + 3 * n * n + n) / 6;
    
    printf("%d\n", sum2 * sum2 - sum1);
    return 0;
}
```

[正整数的平方和公式是什么？怎么证明？](https://www.zhihu.com/question/32253765)

