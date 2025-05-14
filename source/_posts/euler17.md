---
title: 欧拉计划17
date: 2020-08-08 09:57:29
tags:
- C++
- Euler project
categories:
- course
---



## **Number letter counts**

If the numbers 1 to 5 are written out in words: one, two, three, four, five, then there are 3 + 3 + 5 + 4 + 4 = 19 letters used in total.

If all the numbers from 1 to 1000 (one thousand) inclusive were written out in words, how many letters would be used?

**NOTE:** Do not count spaces or hyphens. For example, 342 (three hundred and forty-two) contains 23 letters and 115 (one hundred and fifteen) contains 20 letters. The use of “and” when writing out numbers is in compliance with British usage.



---



```c++
#include <iostream>

int num1[20] = { 0, 3, 3, 5, 4, 4, 3, 5, 5, 4, 
				 3, 6, 6, 8, 8, 7, 7, 9, 8, 8 };

int num2[10] = { 0, 0, 6, 6, 5, 5, 5, 7, 6, 6 };

int get_letter(int num) {
	if (num < 20) { return num1[num]; }
	if (num < 100) { return num2[num / 10] + num1[num % 10]; }
	if (num < 1000) {
		if (num % 100 == 0) { return num1[num / 100] + 7; }
		else { return num1[num / 100] + 10 + get_letter(num % 100); }
	}
	return 11;
}

int main() {
	int res = 0;
	for (int i = 1; i <= 1000; i += 1) {
		res += get_letter(i);
	}

	std::cout << res << std::endl;
	return 0;
}
```

