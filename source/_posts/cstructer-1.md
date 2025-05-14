---
title: C结构体
date: 2020-08-22 14:36:00
tags:
    - C
    - struct
categories:
    - [Dev, c++]
---



# C结构体

C的结构体用于设计复杂类型。例如：

```c++
struct person {
    char name[20];
    int age;
    char gender;
    float height;
};
```

其中`struct person`为变量名，而并非`person`。

`.`为直接引用符，`->`为间接引用符。



# 分配空间原则

**默认情况下：**

1.  找对齐标准
    先找一个占用空间最大的元素。例如在`person`中，最大的是`int`或`float`类型（4字节），4字节就是对其因素，每次开辟4个字节。`name`是字符串，不考虑。
2.  给每一个元素开辟空间。
    `name`占20个字节，开5次，
    `gender`占1个字节，但也分配4个字节。

**使用宏：**





**例：**

```c++
struct node1 {
    char a;
    char b;
    int c;
};


struct node2 {
    char a;
    int c;
    char b;
};
```

以4字节为单位，`node1`先开辟4字节，可以放入`a`，`b`，之后再开辟4字节，放入`c`，总共占用8字节。

同样以4字节为单位，`node2`先开辟4字节，仅可放入`a`，再开辟4字节，放入`c`，最后还需4字节放入`b`，总共占用12字节。





# 共用体

```c++
union register {
    struct {
        unsigned char byte1;
        unsigned char byte2;
        unsigned char byte3;
        unsigned char byte4;
    } bytes;
    unsigned int number;
}
```

与结构体不同，共用体占用空间为占用空间最大元素的空间，上述共用体中，共占用4字节。如果改变byte1的内容，则number的内容也会改变。

例如：

```c++
union node {
    double a;
    char b;
    int c;
}
```

占用情况：

| a    | b    | c    |
| ---- | ---- | ---- |
| 0    | 0    | 0    |
| 1    |      | 1    |
| 2    |      | 2    |
| 3    |      | 3    |
| 4    |      |      |
| 5    |      |      |
| 6    |      |      |
| 7    |      |      |





# 判断大端机、小端机

```c++
int is_little() {
    static int num = 1;
    return ((char*)&num)[0];
}
```

