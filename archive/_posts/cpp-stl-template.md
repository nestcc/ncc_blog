---
title: C++STL标准库实现1——模板编程
date: 2020-07-28 13:53:20
tags:
    - Development
    - C++
categories:
    - Dev
---



# 模板编程



## 模板的基本语法

```c++
template <typename/class T>
```

template 告诉编译器，接下来是一个模板 ，typename 和 class 都是关键字，在这里二者可以互用没有区别。在`< >`中 `T` 叫做模板形参，一旦模板被实例化，`T` 也会变成具体的类型。



## 模板函数

``` c++
template <typename T>
T  add(const T lva ,const T rva)
{

    T a ;

    a = lva + rva ;

return a;
}
```

使用：

``` c++
int add(const int lva ,const int rva)
{

    int a ;

    a = lva + rva ;

return a;
}
```

``` c++
double add(const double lva ,const double rva)
{

    double a ;

     a = lva + rva ;

return a;
}
```

>   如果我们使用`add(1,2.0)`是会报错的，编译器无法找到`add(int,double)`。



## 类模板

``` c++
template <class T>
class Myclass
{
    T a;
    public:
        T add(const T lva ,const T rva);
};

template <class T>
T Myclass<T>::add(const T lva, const T rva)
{
    a = lva + rva;
    return a;
}
```

在程序中给出模板并不能使用它，还必须实例化，比如：

``` c++
Myclass<int> A； //用 int 实例化一个类 A
Myclass<double> B； //用 double 实例化一个类 B
```



## 成员模板：

``` c++
 template <class T>
 class Myclass
 {
     public:
        T a;
        template <typename type_1 , typename type_2>
         type_1 add(const type_1 lva ,const type_2 rva);
 };

 template <class T>
     template <typename type_1,typename type_2>
 type_1 Myclass<T>::add(const type_1 lva, const type_2 rva)
 {
     a = lva + rva;
     return a;
 }
```

```c++
 template <class T>
 class Myclass
 {
     public:
        T a;
        template <typename type_1 , typename type_2>
         type_1 add(const type_1 lva ,const type_2 rva);

         template <class type_3>
         class Myclass_2;         // 声明放在这里，具体定义放在类外进行。
         Myclass_2<T> C;          // 定义一个Myclass_2 类 A。使用 T 进行实例化
 };

 template <class T>
     template <typename type_1,typename type_2>
 type_1 Myclass<T>::add(const type_1 lva, const type_2 rva)
 {
     a = lva + rva;
     return a;
 }

 template <class T>
     template <class type_3>
     class Myclass<T>::Myclass_2
     {
         public:
             type_3 value;
             type_3 sub(const type_3 a , const type_3 b) {vlaue = a - b;}
     };
```



## typename和class的区别

`	typename`和`class`是模板中经常使用的两个关键词 ，在模板定义的时候没有什么区别。以前用的是 class，后来 c++ 委员会加入了 typename。**因为历史原因，两个是可以通用的。**

例子：

``` c++
class Myclass{
    public:
        Myclass();
        typedef int test;  //定义类型别名
}
template <class T>
class Myclass2{
    public:
        Myclass2();
        T::test *a  // 声明一个指向T::test类型的指针。
        //   typename T::test * a
}
```

不能通过编译，因为在 c++ 中，允许我们在类中定义一个类型别名，且使用的时候和类名访问类成员的方法一样。这样编译器在编译的时候就会产生二义性，它根本不知道这是一个类型还是别名，所以我们加上 typename 显式说明出来。

当然如果这里没有二义性，比如`Myclass ::test * a` ,加上 typename 是会报错的。