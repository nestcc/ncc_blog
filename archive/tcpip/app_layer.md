---
title: TCP/IP 应用层
date: 2020-06-17 20:40:42
categories: 
- Dev
---



# 网络应用程序体系结构

* CS
* P2P



## P2P特点

* 两方为对等方，具有良好的扩展性
* 挑战
  * ISP友好
  * 安全性
  * 合适的激励



# 进程与计算机网络之间的接口

* socket是网络与应用程序之间的API
* 应用程序对网络的控制仅限于`选择协议`和`设定参数`



# 可供应用程序使用的运输服务

* 可靠数据传输
* 吞吐量
* 定时
* 安全性



# HTTP

**请求-响应行为**

![Snipaste_2020-12-12_14-04-20](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-12_14-04-20.png)



## 特点

1. 不用担心丢失数据，也不用关注TCP在网络中的数据丢失和乱序等故障。
2. 无状态协议，服务器不需要存储关于客户的状态信息。
3. 服务器总处于打开状态，具有一个固定的IP。
4. 使用了CS结构。



## 持续链接和非持续链接

* HTTP客户端进程发起一个到服务区的TCP链接，使用默认端口80，在服务器上和客户端都有一个socket与之对应。
* HTTP客户端经它的socket向服务器发送请求报文。报文中包好了要请求的对象。
* HTTP服务器接收该报文，从服务器上检索对象，在HTTP响应报文中封装该对象，并通过socket发送。
* HTTP服务器通知TCP断开TCP连接。
* HTTP客户端收到响应报文，TCP链接关闭。
* 对于多个对象重复以上步骤。



## 请求方法

* GET
* POST
* HEAD
* PUT
* DELETE



# HTTPS

![Snipaste_2020-12-14_18-39-48](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-14_18-39-48.png)

* HTTPS需要到CA申请证书。   
* HTTP信息是明文传输，HTTPS是具有安全性的ssl加密传输协议。
* HTTP和HTTPS使用的是完全不同的链接方式，使用的端口也不一样，前者是80，后者是443。
* HTTP是简单的、无状态的链接。
* HTTPS是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比HTTP更安全。





# WEB缓存器（代理服务器）

![Snipaste_2020-12-14_14-36-43](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-14_14-36-43.png)



## WEB缓存器的优点

* 大大减少对客户请求的响应时间。
* 减少了机构的接入链路到英特网的通信量。
* 从整体上大大减少了英特网上的web流量，改善所有应用的性能。



# FTP 



![image-20201205142307455](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/image-20201205142307455.png)



## 双链接

![Snipaste_2020-12-05_14-24-09](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-05_14-24-09.png)



## 短链接的好处

可以复用控制链接。





# SMTP、POP3、IMAP协议

![Snipaste_2020-12-05_14-39-03](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-05_14-39-03.png)



# DNS



## 集中式DNS可能遇到的问题

1. 单点故障
2. 通信容量大
3. 远距离集中式数据库，时延高
4. 维护困难




## DNS记录

(Name, Value, Type, TTL)

1. Type = A : name 主机名, value为ip
2. Type = NS : name 域, value 为权威DNS服务器的主机名
3. Type = CNAME : name为主机别名, value是个规范主机名
4. Type = MX : name是邮件主机别名, value是规范主机名

