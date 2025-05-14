---
title: 线性筛
date: 2020-08-18 15:03:50
tags:
    - C++
    - Primes
categories:
    - [Dev, Algorithm]
---



# 质数筛缺陷



# 线性筛思想

线性筛的总体思想是用一个整数`M`去标记合数`N`，其中`N`和`M`具有如下性质

1.  `N`中最小质数为`P`
2.  `N`可以表示为`P*M`
3.  `P`一定小于等于`M`中最小的因子
4.  利用`M`（所有不大于M中最小的质数）去标记`P*M`



```c++
#define N 30

int nums[N + 5];

int init() {
	for (int i = 2; i < N; i += 1) {
		if (!nums[i]) { nums[++nums[0]] = i; }

		for (int j = 1; j <= nums[0]; j += 1) {
			if (nums[j] * i >= N) { break; }
			nums[nums[j] * i] = nums[j];
			if (i % nums[j] == 0) { break; }
		}
	}
	return 1;
}
```



# 需要判断质数的情况

在`nums`数组中，前`nums[0]`项已经被用于存放质数，因此我们在需要判断一个数字是否是质数的时候无法通过`nums`数组判断，因此我们需要另外一个prime数组。

```c++
int nums[N];
int prime[N] = { 1, 1, 0 };	// 务必将前三项重置为1，1，0

void init() {
    for (int i = 2; i < N; i += 1) {
        if (!nums[i]) { nums[++nums[0]] = i; }
        for (int j = 1; j <= nums[0] ; j += 1) {
            if (nums[j] * i >= N) { break; }
            nums[nums[j] * i] = i;
            prime[nums[j] * i] = i;
            if (i % nums[j] == 0) { break; }
        }
    }
    return ;
}
```

