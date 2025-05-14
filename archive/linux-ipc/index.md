---
title: Linux进程管理
date: 2020-10-31 14:27:19
categories:
- linux
tags:
- linux
- shell
---



# Linux 进程管理

| 命令   | 说明                             | 命令  | 说明                              |
| ------ | -------------------------------- | ----- | --------------------------------- |
| free   | 打印系统情况和内存情况           | top   | 显示当前系统进程、CPU、内存等情况 |
| dstat  | 实时监控系统磁盘、内存、网络情况 | ps    | 报告当前进程状态                  |
| pstree | 以树状显示进程派生关系           | pgrep | 查找进程ID                        |
| kill   | 删除执行中的程序和工作           | pkill | 批量按照进程名杀死进程            |



# dstat

```shell
dstat
dstat 2					# 2秒一次
dstat 2 10			# 2秒一次，一共10次
dstat -list
dstat 2 10 --output ... .csv
```



# ps

```shell
ps -aux
ps -ef
```



# pstree

```shell
pstree -[acGhlnpuU]
```

* \-a : 每个程序的完整指令
* -n : uid排序，计数
* \-p : 显示pid
* \-u : 显示用户名
* \-l : 使用长列格式显示



# 进程间通信

1. file-based: 最开始学的两个进程A和B通过一个文件来写和读。这种方式是通过read/write，需要把进程A的app buffer写到kernel buffer，然后再写到磁盘，然后又从磁盘写到kernel buffer，又从kernel bufferr写到进程B的buffer，所以比较慢。但它可以支持大规模的数据读写，因为磁盘文件可以很大。
2. shm-based: 后来学的share memory通信是进程A和B通过某一段公用内存来读写， 速度比较快，但不能支持大规模的数据读写。因为内存有限。
3. mmap-based: 再后来学的基于mmap的共享内存通信本质上还是通过磁盘上的文件来交换数据，但是不用到read/write函数，而是通过mmap把一个磁盘文件映射到公共内存后，进程A的app buffer的数据直接写到这个公共内存对应的磁盘文件，进程B会直接从这个公共内存对应的磁盘文件读数据到进程B的app buffer，两边都不需要经过kernel buffer，所以速度比file-based方法快，但比shm-based方法慢(因为它本质上还是要读写磁盘)。它也可以支持大规模的数据读写。

速度: file-based < mmap-based < shm-based
支持通信数据大小： file-based = mmap-based > shm-based





## 一、管道

**管道**，通常指无名管道，是 UNIX 系统IPC最古老的形式。

### 1、特点：

1. 它是半双工的（即数据只能在一个方向上流动），具有固定的读端和写端。
2. 它只能用于具有亲缘关系的进程之间的通信（也是父子进程或者兄弟进程之间）。
3. 它可以看成是一种特殊的文件，对于它的读写也可以使用普通的read、write 等函数。但是它不是普通的文件，并不属于其他任何文件系统，并且只存在于内存中。

### 2、原型：

```c
1 #include <unistd.h>
2 int pipe(int fd[2]);    // 返回值：若成功返回0，失败返回-1
```

当一个管道建立时，它会创建两个文件描述符：`fd[0]`为读而打开，`fd[1]`为写而打开



## 二、FIFO

**FIFO**，也称为命名管道，它是一种文件类型。

### 1、特点

1. FIFO可以在无关的进程之间交换数据，与无名管道不同。
2. FIFO有路径名与之相关联，它以一种特殊设备文件形式存在于文件系统中。

### 2、原型

```c
1 #include <sys/stat.h>
2 // 返回值：成功返回0，出错返回-1
3 int mkfifo(const char *pathname, mode_t mode);
```

其中的 mode 参数与`open`函数中的 mode 相同。一旦创建了一个 FIFO，就可以用一般的文件I/O函数操作它。

当 open 一个FIFO时，是否设置非阻塞标志（`O_NONBLOCK`）的区别：

- 若没有指定`O_NONBLOCK`（默认），只读 open 要阻塞到某个其他进程为写而打开此 FIFO。类似的，只写 open 要阻塞到某个其他进程为读而打开它。
- 若指定了`O_NONBLOCK`，则只读 open 立即返回。而只写 open 将出错返回 -1 如果没有进程已经为读而打开该 FIFO，其errno置ENXIO。





## 三、消息队列

**消息队列**，是消息的链接表，存放在内核中。一个消息队列由一个标识符（即队列ID）来标识。

### 1、特点

1. 消息队列是面向记录的，其中的消息具有特定的格式以及特定的优先级。
2. 消息队列独立于发送与接收进程。进程终止时，消息队列及其内容并不会被删除。
3. 消息队列可以实现消息的随机查询,消息不一定要以先进先出的次序读取,也可以按消息的类型读取。

### 2、原型

```c
1 #include <sys/msg.h>
2 // 创建或打开消息队列：成功返回队列ID，失败返回-1
3 int msgget(key_t key, int flag);
4 // 添加消息：成功返回0，失败返回-1
5 int msgsnd(int msqid, const void *ptr, size_t size, int flag);
6 // 读取消息：成功返回消息数据的长度，失败返回-1
7 int msgrcv(int msqid, void *ptr, size_t size, long type,int flag);
8 // 控制消息队列：成功返回0，失败返回-1
9 int msgctl(int msqid, int cmd, struct msqid_ds *buf);
```

在以下两种情况下，`msgget`将创建一个新的消息队列：

- 如果没有与键值key相对应的消息队列，并且flag中包含了`IPC_CREAT`标志位。
- key参数为`IPC_PRIVATE`。

函数`msgrcv`在读取消息队列时，type参数有下面几种情况：

- `type == 0`，返回队列中的第一个消息；
- `type > 0`，返回队列中消息类型为 type 的第一个消息；
- `type < 0`，返回队列中消息类型值小于或等于 type 绝对值的消息，如果有多个，则取类型值最小的消息。



## 四、信号量

**信号量（semaphore）**与已经介绍过的 IPC 结构不同，它是一个计数器。信号量用于实现进程间的互斥与同步，而不是用于存储进程间通信数据。

### 1、特点

1. 信号量用于进程间同步，若要在进程间传递数据需要结合*共享内存*。
2. 信号量基于操作系统的 PV 操作，程序对信号量的操作都是原子操作。
3. 每次对信号量的 PV 操作不仅限于对信号量值加 1 或减 1，而且可以加减任意正整数。
4. 支持信号量组。

### 2、原型

最简单的信号量是只能取 0 和 1 的变量，这也是信号量最常见的一种形式，叫做**二值信号量（Binary Semaphore）**。而可以取多个正整数的信号量被称为通用信号量。

Linux 下的信号量函数都是在通用的信号量数组上进行操作，而不是在一个单一的二值信号量上进行操作。

```c
1 #include <sys/sem.h>
2 // 创建或获取一个信号量组：若成功返回信号量集ID，失败返回-1
3 int semget(key_t key, int num_sems, int sem_flags);
4 // 对信号量组进行操作，改变信号量的值：成功返回0，失败返回-1
5 int semop(int semid, struct sembuf semoparray[], size_t numops);  
6 // 控制信号量的相关信息
7 int semctl(int semid, int sem_num, int cmd, ...);
```

当`semget`创建新的信号量集合时，必须指定集合中信号量的个数（即`num_sems`），通常为1； 如果是引用一个现有的集合，则将`num_sems`指定为 0 。

在`semop`函数中，`sembuf`结构的定义如下：

```c
1 struct sembuf 
2 {
3     short sem_num; // 信号量组中对应的序号，0～sem_nums-1
4     short sem_op;  // 信号量值在一次操作中的改变量
5     short sem_flg; // IPC_NOWAIT, SEM_UNDO
6 }
```

其中 sem_op 是一次操作中的信号量的改变量：

- 若`sem_op > 0`，表示进程释放相应的资源数，将 sem_op 的值加到信号量的值上。如果有进程正在休眠等待此信号量，则换行它们。

- 若`sem_op < 0`，请求 sem_op 的绝对值的资源。

  - 如果相应的资源数可以满足请求，则将该信号量的值减去sem_op的绝对值，函数成功返回。

  - 当相应的资源数不能满足请求时，这个操作与

    ```
    sem_flg
    ```

    有关。

    - sem_flg 指定`IPC_NOWAIT`，则semop函数出错返回`EAGAIN`。

    - sem_flg 没有指定

      ```
      IPC_NOWAIT
      ```

      ，则将该信号量的semncnt值加1，然后进程挂起直到下述情况发生：

      1. 当相应的资源数可以满足请求，此信号量的semncnt值减1，该信号量的值减去sem_op的绝对值。成功返回；
      2. 此信号量被删除，函数smeop出错返回EIDRM；
      3. 进程捕捉到信号，并从信号处理函数返回，此情况下将此信号量的semncnt值减1，函数semop出错返回EINTR

- 若`sem_op == 0`，进程阻塞直到信号量的相应值为0：

  - 当信号量已经为0，函数立即返回。

  - 如果信号量的值不为0，则依据

    ```
    sem_flg
    ```

    决定函数动作：

    - sem_flg指定`IPC_NOWAIT`，则出错返回`EAGAIN`。

    - sem_flg没有指定

      ```
      IPC_NOWAIT
      ```

      ，则将该信号量的semncnt值加1，然后进程挂起直到下述情况发生：

      1. 信号量值为0，将信号量的semzcnt的值减1，函数semop成功返回；
      2. 此信号量被删除，函数smeop出错返回EIDRM；
      3. 进程捕捉到信号，并从信号处理函数返回，在此情况将此信号量的semncnt值减1，函数semop出错返回EINTR

在`semctl`函数中的命令有多种，这里就说两个常用的：

- `SETVAL`：用于初始化信号量为一个已知的值。所需要的值作为联合semun的val成员来传递。在信号量第一次使用之前需要设置信号量。
- `IPC_RMID`：删除一个信号量集合。如果不删除信号量，它将继续在系统中存在，即使程序已经退出，它可能在你下次运行此程序时引发问题，而且信号量是一种有限的资源。



## 五、共享内存

**共享内存（Shared Memory）**，指两个或多个进程共享一个给定的存储区。

### 1、特点

1. 共享内存是最快的一种 IPC，因为进程是直接对内存进行存取。
2. 因为多个进程可以同时操作，所以需要进行同步。
3. 信号量+共享内存通常结合在一起使用，信号量用来同步对共享内存的访问。

### 2、原型

```
1 #include <sys/shm.h>
2 // 创建或获取一个共享内存：成功返回共享内存ID，失败返回-1
3 int shmget(key_t key, size_t size, int flag);
4 // 连接共享内存到当前进程的地址空间：成功返回指向共享内存的指针，失败返回-1
5 void *shmat(int shm_id, const void *addr, int flag);
6 // 断开与共享内存的连接：成功返回0，失败返回-1
7 int shmdt(void *addr); 
8 // 控制共享内存的相关信息：成功返回0，失败返回-1
9 int shmctl(int shm_id, int cmd, struct shmid_ds *buf);
```

当用`shmget`函数创建一段共享内存时，必须指定其 size；而如果引用一个已存在的共享内存，则将 size 指定为0 。

当一段共享内存被创建以后，它并不能被任何进程访问。必须使用`shmat`函数连接该共享内存到当前进程的地址空间，连接成功后把共享内存区对象映射到调用进程的地址空间，随后可像本地空间一样访问。

`shmdt`函数是用来断开`shmat`建立的连接的。注意，这并不是从系统中删除该共享内存，只是当前进程不能再访问该共享内存而已。

`shmctl`函数可以对共享内存执行多种操作，根据参数 cmd 执行相应的操作。常用的是`IPC_RMID`（从系统中删除该共享内存）。