> 绿为发送方， 蓝为接收方

# RDT 1.0 完全可靠信道

![Snipaste_2020-12-06_22-10-34](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-10-34.png)



# RDT 2.0 具有比特差错信道

![Snipaste_2020-12-06_22-11-39](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-11-39.png)



checksnum ：校验和



![Snipaste_2020-12-06_22-13-38](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-13-38.png)



RDT2.0每次只能发一个包， 效率低下



# RDT 2.1 

> 加入了0、1包

![Snipaste_2020-12-06_22-19-41](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-19-41.png)

![Snipaste_2020-12-06_22-24-15](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-24-15.png)



## 0、1包的好处

可以清楚地得知每次接收的NAK、ACK所属的数据包。



# RDT 2.2 

> 去除了NAK，默认未收到ACK则为NAK。

![Snipaste_2020-12-06_22-27-59](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-27-59.png)

![Snipaste_2020-12-06_22-30-39](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-30-39.png)





# RDT 3.0 

> 加入了定时器

![Snipaste_2020-12-06_22-33-50](/Users/nestcc/project/ncc_blog/source/_posts/tcpip-1/Snipaste_2020-12-06_22-33-50.png)

接收方无改变



## 存在的问题

序号太少，可能收到上一个循环返回的ACK，造成冲突。