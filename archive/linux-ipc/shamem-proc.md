---
title: 基于共享内存的进程间通讯方式
date: 2020-11-21 21:10:43
categories:
- linux
tags:
- process
- linux
---





# 所需接口

1. shmget





# shmget

## 作用

获得一块System V的共享内存段。

## 使用

```c++
#include <sys/ipc.h>
#include <sys/shm.h>

int shmget(key_t key, size_t size, int shmflg);
```

key_t : 关键词类型

size : 所需空间大小

shmflg : 申请时的flag

return : 返回共享内存段id



