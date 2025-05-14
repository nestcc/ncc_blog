---
title: day 2
date: 2021-02-12
categories:
- Tohka
---



# UTF-8编码规则

UTF-8是一种变长编码，它可以使用1～4个字节表示一个符号，根据不同的符号而变化字节长度。其编码规则有两条：

1.  对于单字节的符号，字节第`1`位为`0`，后面`7`位为这个字符的unicode码。因此对于英文字母来说，UTF-8编码和ACSII码是相同的。
2.  对于`n`字节的符号（n>1)，第一字节的前`n`位都为1，第`n+1` 位为0，之后字节的前`2`位为`10`。剩下的没有提及的二进制位全部为这个符号的unicode码。

| Unicode 符号范围（十六进制） | UTF-8编码方式（二进制）                 |
| ---------------------------- | --------------------------------------- |
| 0000 0000 - 0000 007F        | 0xxx xxxx                               |
| 0000 0080 - 0000 07FF        | 110x xxxx 10xx xxxx                     |
| 0000 0800 - 0000 FFFF        | 1110 xxxx 10xx xxxx 10xx xxxx           |
| 0001 0000 - 0010 FFFF        | 1111 0xxx 10xx xxxx 10xx xxxx 10xx xxxx |



# unicode.h

>   实现unicode的编码和解码功能

```c++
#ifndef _UNICODE_H_
#define _UNICODE_H_

uint32_t get_number_encode_utf8(int value);
uint32_t get_number_decode_utf8(uint8_t byte);
uint8_t encode_utf8(uint8_t *buff, int value);
int decode_utf8(const uint8_t *byte_ptr, size_t length);

#endif

```



# unicode.cpp

```c++
#include <cinttypes>
#include <iostream>

#include "common.h"
#include "unicode.h"


uint32_t get_number_encode_utf8(int value) {
    ASSERT(value > 0, "Can`t encode negative value!");

    if (value <= 0x7f) {
        return 1;
    }

    if (value <= 0x7ff) {
        return 2;
    }

    if (value <= 0xffff) {
        return 3;
    }

    if (value <= 0x10ffff) {
        return 4;
    } 

    return 0;
}

uint8_t encode_utf8(uint8_t *buff, int value) {
    ASSERT(value > 0, "Can`t encode negative value!");

    if (value <= 0x7f) {
        *buff = value & 0x7f;
        return 1;
    } else if (value <= 0x7ff) {
        *buff ++ = 0xc0 | ((value & 0x7c0) >> 6);
        *buff = 0x80 | (value & 0x3f);
        return 2;
    } else if (value <= 0xffff) {
        *buff ++ = 0xe0 | ((value & 0xf000) >> 12);
        *buff ++ = 0x80 | ((value & 0xfc0) >> 6);
        *buff = 0x80 | (value & 0x3f);
        return 3;
    } else if (value <= 0x10ffff) {
        *buff ++ = 0xf0 | ((value & 0x1c0000) >> 18);
        *buff ++ = 0x80 | ((value & 0x3f000) >> 12);
        *buff ++ = 0x80 | ((value & 0xfc0) >> 6);
        *buff = 0x80 | (value & 0x3f);
        return 4;
    }

    NOT_REACHED();
    return 0;
}

uint32_t get_number_decode_utf8(uint8_t byte) {
    if ((byte & 0xc0) == 0x80) {
        return 0;
    }

    if ((byte & 0xf8) == 0xf0) {
        return 4;
    }

    if ((byte & 0xf0) == 0xe0) {
        return 3;
    }

    if ((byte & 0xe0) == 0xc0) {
        return 2;
    }

    return 1;   //ascii码
} 

int decode_utf8(const uint8_t *byte_ptr, size_t length) {
    if (*byte_ptr <= 0x7f) {
        return *byte_ptr;
    }

    int value;
    uint32_t remainingBytes;

    if ((*byte_ptr & 0xe0) == 0xc0) {
        value = *byte_ptr & 0x1f; 
        remainingBytes = 1;
    } else if ((*byte_ptr & 0xf0) == 0xe0) {
        value = *byte_ptr & 0x0f; 
        remainingBytes = 2;
    } else if ((*byte_ptr & 0xf8) == 0xf0) {
        value = *byte_ptr & 0x07; 
        remainingBytes = 3;
    } else {
        return -1;  
    }

    if (remainingBytes > length - 1) {
        return -1;
    }

    while (remainingBytes > 0) {
        byte_ptr++;  
        remainingBytes--;
        if ((*byte_ptr & 0xc0) != 0x80) {
	        return -1;
        }
        value = value << 6 | (*byte_ptr & 0x3f);
    }
    return value;
}


```

