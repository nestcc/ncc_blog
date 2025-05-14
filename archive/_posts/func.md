---
title: C++ 函数
date: 2020-08-06 10:54:28
tags:
    - C++
    - function
categories:
    - [Dev, c++]
---



# 函数指针

把函数当作参数传入方程中。



```c++
int g(int (*f1)(int), int (*f2)(int), int (*f3)(int)) {
    if (x < 0) {
        return f1(x);
    }
    if (x < 100) {
        return f2(x);
    }
    return f3(x);
}
```



# 递归



1.  边界条件
2.  处理过程，递归过程
3.  返回



#### 与数学归纳法类似

1.  f(0) 成立							 ==>	边界条件成立
2.  f(k)成立，f(k+1)成立        ==>    处理过程



# 变参函数

*   需要引入\<cstdarg\>头文件

```c++
int func(int a, ...);
```

*   va_list                                        => 变参列表
*   va_start()                                   => a后面的第一个参数
*   va_arg()                                     => 下一个可变参数列表中的参数
*   va_end()                                    => 结束获取可变参数列表的动作

例：

``` c++
int max_int(int n, ...) {
    int ans = 0;
    va_list arg;
    va_start(arg, n);
    while (n--) {
        int tmp = va_arg(arg, int);
        if (tmp > ans) ans = tmp;
    }
    va_end(arg);
    
    return ans;
}

int main() {
    max_int(3, 1, 5, 3);
    // ...
    return 0;
}
```

