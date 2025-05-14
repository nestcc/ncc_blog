---
title: day 2
date: 2021-02-12
categories:
- Tohka
---



>   实现VM虚拟机结构



# vm.h

>   虚拟机类

```c++
class VM {
    uint32_t allocated_byte;
    Parser *curr_parser;

public:
    VM();
};
```



## 成员变量

1.  allocated_byte：已经分配出去的字节数
2.  curr_parser：语法分析器指针



# core.h

>   用于读取代码源文件

```c++
#ifndef _CORE_H_
#define _CORE_H_


extern char *root_dir;
char *read_file(const char *fname);

#endif
```





# core.cpp

>   很简单的读取文件功能

```c++
#include <string>
#include <sys/stat.h>
#include "common.h"
#include "utils.h"
#include "core.h"

char *root_dir = nullptr;

// 读源码文件
char *read_file(const char *fpath) {
    FILE *srcf = fopen(fpath, "r");

    if (srcf == nullptr) {
        IO_ERROR(RED "Cannot open file %s.\n" NOCOLOR, fpath);
        exit(EXIT_FAILURE);
    }

    struct stat file_status;
    stat(fpath, &file_status);

    size_t fsize = file_status.st_size;

    char *fcontent = (char *) malloc(sizeof(char) * (fsize + 5));

    if (fcontent == nullptr) {
        MEM_ERROR(RED "Allocate memory for source file %s failed.\n" NOCOLOR, fpath);
        exit(EXIT_FAILURE);
    }


    size_t read_size = fread(fcontent, sizeof(char), fsize, srcf);

    if (read_size < fsize) {
        IO_ERROR(RED "Read file failed, total %d, get %d.\n" NOCOLOR, fsize, read_size);
        exit(EXIT_FAILURE);
    }

    fcontent[fsize] = EOF;

    fclose(srcf);

    return fcontent;

}

```



