---
title: 仿Google Test测试框架——1
categorie: 
    - [Dev, C++, Test Project]
tags: 
    - C++
    - test
date: 2020-10-22 16:24:15
---



# 1. 何为Google Test

> Google Test是基于xUnit架构的C ++编程语言的单元测试库。该库是根据BSD 3条款许可发布的。它可以针对各种POSIX和Windows平台进行编译，允许对C源代码和C ++源代码进行单元测试，而只需对源代码进行最少的修改。这些测试本身可以一次运行一个，甚至可以一次调用全部运行。(维基百科)



# 2. 如何安装及使用

1. Google test 没有即开即用的版本，使用需要先使用cmake编译原项目才能使用，故我们需先准备cmake环境。

2. 之后克隆GitHub仓库：

   ```bash
   git clone https://github.com/google/googletest.git
   ```
   
3. 之后运行如下命令进行编译
   
   ```bash
   mkdir mybuild       # Create a directory to hold the build output.
   cd mybuild
   cmake ${GTEST_DIR}  # Generate native build scripts.
   ```
   
4. 安装完成。。。



# 3. 如何使用Google Test

1. 需要在任意一段c++代码中引入头文件<gtest/gtest.h>。
2. 使用TEST()宏创建测试用例。
3. 在主函数中调用RUN_ALL_TESTS()函数。

如下：

```c++
#include <iostream>
// using namespace std;
#include <gtest/gtest.h>

int add(int a, int b){
    return a +b;
}

TEST(testCase, test0){
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(3, 4), 7);
    ASSERT_EQ(add(1, 3), 8);
    EXPECT_EQ(add(2, 4), 6);
}


int main(int argc, char **argv){
  testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
```

编译运行后就可得到如下结果：

```shell
[==========] Running 1 test from 1 test suite.
[----------] Global test environment set-up.
[----------] 1 test from testCase
[ RUN      ] testCase.test0
first.cpp:19: Failure
Expected equality of these values:
  add(1, 3)
    Which is: 4
  8
[  FAILED  ] testCase.test0 (0 ms)
[----------] 1 test from testCase (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test suite ran. (0 ms total)
[  PASSED  ] 0 tests.
[  FAILED  ] 1 test, listed below:
[  FAILED  ] testCase.test0

 1 FAILED TEST
```



