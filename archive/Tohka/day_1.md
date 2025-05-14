---
title: 第一天
time: 2021-03-09
categories:
- Tohka
---



# common.h

>   各模块中通用的宏。



```c++
#define NOCOLOR     "\e[0m"         //清除颜色，即之后的打印为正常输出，之前的不受影响
#define BLACK       "\e[0;30m"      //深黑
#define L_BLACK     "\e[1;30m"      //亮黑，偏灰褐
#define RED         "\e[0;31m"      //深红，暗红
#define L_RED       "\e[1;31m"      //鲜红
#define GREEN       "\e[0;32m"      //深绿，暗绿
#define L_GREEN     "\e[1;32m"      //鲜绿
#define BROWN       "\e[0;33m"      //深黄，暗黄
#define YELLOW      "\e[1;33m"      //鲜黄
#define BLUE        "\e[0;34m"      //深蓝，暗蓝
#define L_BLUE      "\e[1;34m"      //亮蓝，偏白灰
#define PINK        "\e[0;35m"      //深粉，暗粉，偏暗紫
#define L_PINK      "\e[1;35m"      //亮粉，偏白灰
#define CYAN        "\e[0;36m"      //暗青色
#define L_CYAN      "\e[1;36m"      //鲜亮青色
#define GRAY        "\e[0;37m"      //灰色
#define WHITE       "\e[1;37m"      //白色，字体粗一点，比正常大，比bold小
#define BOLD        "\e[1m"         //白色，粗体
#define UNDERLINE   "\e[4m"         //下划线，白色，正常大小
#define BLINK       "\e[5m"         //闪烁，白色，正常大小
#define REVERSE     "\e[7m"         //反转，即字体背景为白色，字体为黑色
#define HIDE        "\e[8m"         //隐藏
#define CLEAR       "\e[2J"         //清除
#define CLRLINE     "\r\e[K"        //清除行

#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

typedef struct _VM VM;
typedef struct _PARSER Parser;
typedef struct _CLASS Class;

// #define bool char
// #define True 1
// #define False 0
#define UNUSED __attribute__((unused))

#ifdef DEBUG
#define ASSERT(condition, errmsg)    \
do {    \
    if (!condition) {   \
        fprintf(stderr, RED "An error occured!\n" NOCOLOR "%s at : %s,%d:%s()\n",   \
            errmsg, __FILE__, __LINE__, __func__);  \
        abort();    \
    }   \
} while (0);

#else
#define ASSERT(condition, errmsg)
#endif

#define NOT_REACHED()   \
do {    \
    fprintf(stderr, BROWN "Not reached:\n" NOCOLOR "%s,%d:%s()\n",   \
        __FILE__, __LINE__, __func__);  \
} while (0);

```



1.  VM,Parser,Class宏将在后面实现。
2.  消除编译器对函数内未使用的形参的警报，使用UNUSED宏。
3.  ASSERT实现调试断点。
4.  如果一些分支是不可到达而实际运行时到达了的，说明该段程序出错，将UN_REACHED宏安插到代码中以检测错误。





# utils.h

>   用于实现内存管理功能



```c++
#include "common.h"

void *mem_manager(VM *vm, void *ptr, uint32_t old_size, uint32_t new_size);

#define ALLOCATE(vm_ptr, type) \
(type *) men_manager(vm_ptr, NULL, 0, sizeof(type));

#define ALLOCATE_EXTRA(vm_ptr, type, extra_size) \
(type *) mem_manager(vm_ptr, NULL, 0, sizeof(type) + extra_size);

#define ALLOCATE_ARRAY(vm_ptr, type, capacity) \
(type *) mem_manager(vm_ptr, NULL, 0, sizeof(typr) * capacity);

#define DESTORY_ARRAY(vm_ptr, arr_ptr, capacity) \
mem_manager(vm_ptr, arr_ptr, sizeof(arr_ptr[0]) * capacity, 0);

#define DESTORY(vm_ptr, mem_ptr) \
mem_manager(vm_ptr, mem_ptr, 0, 0);

uint32_t ceil_to_squar(uint32_t v);

struct STRING {
    uint32_t length;
    char *str;
};

struct CharVal {
    uint32_t length;
    char str[0];
};

template<typename Type>
class MemBuffer {
public:
    Type *data;
    
    uint32_t count;

    uint32_t capacity;
    
    MemBuffer() : data(nullptr), count(0) {} ;

    void fillWirte(VM *vm, Type &data, uint32_t fill_cnt);

    void buffAdd(VM *vm, Type &data);

    void buffClear(VM *vm);

};

#define SymbolTable MemBuffer<STRING>

typedef uint8_t Byte;
typedef char Char;
typedef int Int;

enum ErrType {
    ERROR_IO,
    ERROR_MEM,
    ERROR_LEX,
    ERROR_COMPILE,
    ERROR_RUNTIME,
};

void symbol_table_clear(VM *vm, SymbolTable *buffer);

void report_error(void *parser, ErrType err_type, const char *fmt, ...);

#define IO_ERROR(...) \
report_error(NULL, ErrType::ERROR_IO, __VA_ARGS__);

#define MEM_ERROR(...) \
report_error(NULL, ErrType::ERROR_MEM, __VA_ARGS__);

#define LEX_ERROR(parser, ...) \
report_error(parser, ErrType::ERROR_LEX, __VA_ARGS__);

#define COMPILE_ERROR(parser, ...) \
report_error(parser, ErrType::ERROR_COMPILE, __VA_ARGS__);

#define RUNTIME_ERROR(parser, ...) \
report_error(parser, ErrType::ERROR_RUNTIME, __VA_ARGS__);

#define DEFAULT_BUfFER_SIZE 512

```



## mem_manager

用于内存管理，负责开辟和释放内存。

接下来的5个宏为基于`mem_manager` 实现的内存管理宏。

*   ALLOCATE
*   ALLOCATE_EXTRA
*   ALLOCATE_ARRAY
*   DESTORY_ARRAY
*   DESTORY





## STRING CharVal

两种数据类型。

`STRING`用于存储字符串，这是为后面定义的`String`类型缓冲区做准备。

`CharVal`用于存储字符串对象中的字符串，由于字符串对象长度不确定，因此`char str[]`长度为0，类似于c99中的柔性数组。



## MemBuffer

各种数据类型缓冲区的模版类，相当于简版的STL空间适配器。

### 成员变量

1.  Type *data：一个Type类型的数组。
2.  uint32_t count：已经使用元素的个数。
3.  uint32_t capacity：缓冲区总共可存放的元素个数。



### 成员函数

*   void fillWirte(VM *vm, Type &data, uint32_t fill_cnt)

    向数组中连续存入`fill_cnt`个`data`元素，如果空间不够则扩容。每次扩容操作使得原数组容量变为原来的2倍。

*   void buffAdd(VM *vm, Type &data)

    存入一个`data`

*   void buffClear(VM *vm)

    清空缓冲区



## 错误类型

ErrType声明错误类型，用于不同类型的报错。之后是相关的宏定义，都是调用report_error()完成的。



# utilis.cpp

>   实现相关函数



```c++
#include "includes/utils_cpp.h"


uint32_t ceil_to_squar(uint32_t v) {
   v += (v == 0);  
   v--;
   v |= v >> 1;
   v |= v >> 2;
   v |= v >> 4;
   v |= v >> 8;
   v |= v >> 16;
   v++;
   return v;
}

void *mem_manager(VM *vm, void *data, uint32_t old_size, uint32_t new_size) {
    
    // vm -> allocatedBytes += new_size - old_size;

    if (new_size == 0) {
        free(data);
        return nullptr;
    }

    return realloc(data, new_size);
}

```

