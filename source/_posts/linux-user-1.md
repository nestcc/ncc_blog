---
title: Linux用户管理
date: 2020-10-28 14:53:08
categories:
- linux
tags:
- course
- linux
- shell	
---



# 重要配置文件

| 文件         | 内容                                                  |
| ------------ | ----------------------------------------------------- |
| /etc/passwd  | 用户，密码位，用户编号，归组编号，姓名，$HOME，$SHELL |
| /etc/shadow  | 用户，已加密密码，密码改动信息，密码策略              |
| /etc/group   | 群组名，密码位，群组编号，组内用户                    |
| /etc/gshadow | 群组密码相关文件                                      |
| /etc/sudoers | 用户名，权限定义，权限(sudo)                          |



# /etc/passwd

```
root:x:0:0:root:/root:./sh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
```

| root     | x    | 0    | 0    | root     | /root | ./sh   |
| -------- | ---- | ---- | ---- | -------- | ----- | ------ |
| 账号名称 | 密码 | uid  | gid  | 用户信息 | $HOME | $SHELL |



# /etc/shadow

```
root:$6$YUw2gL...//:18472:0:99999:7:::
daemon:*:18113:0:99999:7:::
bin:*:18113:0:99999:7:::
sys:*:18113:0:99999:7:::
sync:*:18113:0:99999:7:::
games:*:18113:0:99999:7:::
```

| root     | $6...//      | 18472          | 0                | 99999              | 7                    | null             | null         | Null |
| -------- | ------------ | -------------- | ---------------- | ------------------ | -------------------- | ---------------- | ------------ | ---- |
| 账户名称 | 加密后的密码 | 修改密码的日期 | 密码不可改动时间 | 密码需重新修改时间 | 密码变更期限提前警告 | 密码过期宽限时间 | 账户失效日期 | 保留 |



# /etc/group

```
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog
tty:x:5:
```

| root | x      | 0    | null     |
| ---- | ------ | ---- | -------- |
| 组名 | 密码位 | gid  | 组内用户 |



# /etc/gshadow

```
root:*::
daemon:*::
bin:*::
sys:*::
adm:*::syslog
```

| root | *    | null   | Null     |
| ---- | ---- | ------ | -------- |
| 组名 | 密码 | 管理员 | 组内用户 |



# chsh 更改用户shell

```bash
chsh -s Shell <username>
```



# 用户管理相关命令

| 命令    | 说明         | 命令    | 说明             |
| ------- | ------------ | ------- | ---------------- |
| su      | 切换用户     | sudo    | 临时切换root用户 |
| passwd  | 设定用户密码 | gpasswd | 设定群组密码     |
| chsh    | 切换shell    | usermod | 修改用户账号     |
| useradd | 新建用户     | userdel | 删除用户         |
| Id      | 显示用户信息 | ...     | ...              |



# su 切换用户

```shell
su [-lmpfc] <username>
```

* \- | \-l : 重新登陆
* \-m | \-p : 不更改环境变量
* \-c command : 切换后执行命令，并退出



# sudo 临时切换到root

```shell
sudo [-siul] <command>
```

* \-s : 切换为root shell
* \-i : 切换到root shell，并初始化
* \-u username | did : 执行命令的身份
* \-l : 显示自己的权限



# passwd 设定用户密码

```shell
passwd [-dleSxnf] <username>
```

* \-d : 清除密码
* \-l : 锁定账户
* \-e : 使密码过期
* \-S : 显示密码认证信息
* \-x days : 密码修改后最大使用天数
* \-n days : 密码修改间隔时间
* \-f : 更改用户信息
* \-i : 密码过期后宽限时间



# gpasswd 设置组群及组群密码

```shell
gpassed [-adrAM] <groupname>
```

* \-a username : 将用户加入组群
* \-d username : 将用户从组群中移除
* \-r : 删除密码
* \-A username : 将用户设置为组群管理员
* \-M username1, username2, ... : 设置组群成员



# useradd 添加用户

```shell
useradd [-dmMsugGnefcD] <username>
```

* \-d dir : 指定$HOME
* \-m : 自动建立$HOME
* \-M : 不自动建立$HOME
* \-s shell : 设置用户登陆shell
* \-u uid : 设置用户id
* \-g groupname : 设置用户归属组群
* \-G groupname : 设置用户归属附加组群
* \-n : 不建立以用户名为组群名称的组群
* \-e days : 设置账号过期时间，绝对时间
* \-f days : 缓冲时间，days天后关闭账号
* \-c string : 设置用户备注
* \-D [exp] : 更改预设值



/etc/login.defs 新建用户规则

/etc/skel 新建用户默认文件



# userdel 删除用户

```shell
userdel -r <username>
```

* \-r : 删除用户相关文件和目录（邮件，家目录）



# usermod 命令

```shell
usermod [-cdefgGlLsuU] <username>
```

* \-l username : 修改用户账号名称
* \-L : 锁定用户密码，使密码无效
* \-s shell : 修改用户登陆后所使用的shell
* \-u did : 修改用户uid
* \-U : 解除密码锁定 



# Id 命令

```shell
id [-gGnru] <username>
```

* \-g : 下属所属组ID
* \-G : 显示附加组ID
* \-n : 显示用户，所属组码，或附加群组的名称
* \-u : 显示用户ID
* \-r : 显示实际ID