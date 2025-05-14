---
title: C++数组与指针
date: 2020-08-13 14:34:50
tag:
    - C++
    - array
    - pointer
categories:
    - Dev
---
# C++中的数组与传参

## 一维数组

我们写下如下代码：

```c++
#include <cstdio>

int main() {
    int arr[10] = { 1, 2, 0 };
    printf("arr = %p, &arr[0] = %p\n", arr, &arr[0]); 
    printf("arr + 1 = %p, &arr[1] = %p, arr + 2 = %p\n", arr + 1, &arr[1], arr + 2);
    char *p = (char *) arr;
    printf("p = %p, p + 1 = %p\n", p, p + 1);
  
    int n = 123;
    printf("sizeof(n) = %lu\n", sizeof(n));
    printf("sizeof(arr) = %lu\n", sizeof(arr));
  
    return 0;
}
```

输出

```
arr = 000000A634B3FA68, &arr[0] = 000000A634B3FA68
arr + 1 = 000000A634B3FA6C, &arr[1] = 000000A634B3FA6C, arr + 2 = 000000A634B3FA70
p = 000000A634B3FA68, p + 1 = 000000A634B3FA69
sizeof(n) = 4
sizeof(arr) = 40
```

可以看到:

* 第一行全部为数组首地址的值，
* 第二行分别为arr[1]和arr[2]的地址，我们可以看出[0]，[1]和[2]的地址相差4，即int类型的size。
* 第三行arr + 1和&arr[1]是一样的，这说明arr[1]和*(arr + 1)是等价的，都可以用于访问arr[1]。

因此我们在传参的时候只需要传数组的首地址即可。

如：

```c++
void func1(int *a) {
    print("a[0] = %d, a[1] = %d", a[0], a[1]);
    ...
}
```

## 二维数组

写下如下代码

```c++
int main() {
    int num[10][2];

    printf("num = %p, num + 1 = %p\n", num, num + 1);

    return 0;
}
```

执行结果

```
num = 0000008797F9F870, num + 1 = 0000008797F9F878
```

说明num + 1是num第二行的首地址。

因此不能使用如下方法传二维数组

```c++
void func(int **p) {
    ...
}
```

需要使用

```c++
void func(int (*p)[3]) {
    ...
}
```

# n维数组

与二维数组类似

```
int num[20][3][2];
```

```c++
void func(int (*p)[3][2]) {
    ...
}
```
