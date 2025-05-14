---
title: 基于文件的进程间通信
date: 2020-11-21 21:10:25
categories:
- linux
tags:
- linux
- process
---







# 需求描述

1. 设置一个并发度`INS`，表示要避开的进程数量

2. 使用`INS`个进程，计算从`start`到`end`之间的数字累加和

   ```shell
   ./add -s 10 -e 100 -i 5
   ```

3. `start`和`end`使用`getopt`解析命令行参数

4. 输出一个整型`sum`



# 注意

1. 使用`文件`进行数据共享，需考虑数据竞争`data race`
2. 尝试使用`文件锁`来模拟线程间的`互斥锁`
3. 通过`文件锁`实现了`临界数据`的同步访问



# 函数和接口

1. flock