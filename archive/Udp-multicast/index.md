---
title: Udp组播
date: 2021-02-03 16:29:06
---



# Udp组播编程

## 函数简介

1. setsockopt函数

```c
  #include <sys/types.h>          /* See NOTES */
  #include <sys/socket.h>
  int setsockopt(int sockfd, int level, int optname,
                      const void *optval, socklen_t optlen);
```



1. level

- IPPROTO_IP

1. optname

- IP_MULTICAST_LOOP 支持多播数据回送
- IP_ADD_MEMBERSHIP 加入多播组
- IP_DROP_MEMBERSHIP 离开多播组

1. optval

- IP_MULTICAST_LOOP 选项对应传入 unsigned int 来确认是否支持多播数据回送
- IP_ADD_MEMBERSHIP 传入 ip_mreq
- IP_DROP_MEMBERSHIP 传入 ip_mreq