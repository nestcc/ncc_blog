---
title: 基于管道的进程间通讯
date: 2020-11-23 09:42:35
categories:
- linux
tags:
- linux 
- process
---





# 所需接口



## popen



```c++
FILE *popen(const char *command, const char *mode);
```

| 参数                | 说明                                   |
| ------------------- | -------------------------------------- |
| command             | shell命令行，会被/bin/sh以-c的形式执行 |
| mode(linux下为type) | 读写模式                               |



## pipe, pipe2



```c++
#include <unistd.h>

int pipe(int pipefd[2]);

#define _GNU_SOURCE             /* See feature_test_macros(7) */
#include <fcntl.h>              /* Obtain O_* constant definitions */
#include <unistd.h>

int pipe2(int pipefd[2], int flags);
```



| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| pipefd | 用于接收返回的文件描述符，一个指向文件开头，另一个指向文件结尾，相当于管道的进出口。0指向读端，1指向写端。 |
| flags  |                                                              |
| 返回值 | 成功则返回0，否则-1，并且errno也会被设置。                   |



## 示例

```c++
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main(int argc, char *argv[]) {
  int pipefd[2];
  pid_t cpid;
  char buf;

  if (argc != 2) {
    fprintf(stderr, "Usage: %s <string>\n", argv[0]);
    exit(EXIT_FAILURE);
  }

  if (pipe(pipefd) == -1) {
    perror("pipe");
    exit(EXIT_FAILURE);
  }

  cpid = fork();
  if (cpid == -1) {
    perror("fork");
    exit(EXIT_FAILURE);
  }
  if (cpid == 0) {    /* Child reads from pipe */
    close(pipefd[1]);          /* Close unused write end */

    while (read(pipefd[0], &buf, 1) > 0)
      write(STDOUT_FILENO, &buf, 1);

    write(STDOUT_FILENO, "\n", 1);
    close(pipefd[0]);
    _exit(EXIT_SUCCESS);

  } else {            /* Parent writes argv[1] to pipe */
    close(pipefd[0]);          /* Close unused read end */
    write(pipefd[1], argv[1], strlen(argv[1]));
    close(pipefd[1]);          /* Reader will see EOF */
    wait(NULL);                /* Wait for child */
    exit(EXIT_SUCCESS);
  }
}
```

