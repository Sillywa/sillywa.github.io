---
title: Linux下使用crontab定时执行任务
date: 2019-07-17 19:40:39
top: 10
tags: 
- crontab
categories:
- Linux
---

## 起因

最近在做一个小项目，要求能定时利用 [pm2](http://doc.pm2.io/en/plus/overview/) 重启某个进程，即定时执行 `pm2 restart xxx`。

今天刚好发现 Linux 下可以使用 crontab 来定时执行一些脚本或命令，于是我就开始研究如何利用 crontab 搭配 pm2 定时重启某个进程。



## 过程

在研究的过程中，我自己在网上看了很多教程，都是良莠不齐，花了好大功夫我才解决这个问题，接下来具体看一下我的解决过程。

首先看一下 Linux 下如何使用 crontab：

```
crontab -l      // 列出Linux下当前用户所有的定时任务
crontab -e     // 编辑定时任务
```

当首次使用 `crontab -l` 时，会提示当前用户下没有定时任务。

这时我们可以使用 `crontab -e` 创建一个定时任务，创建时会要求我们选择编辑器，这里我们选择 vim.tiny。如果第一次编辑器选错了，可以运行 `sudo select-editor`命令重新选择。

{% asset_img 1.png [选择编辑器] %}

然后我们可以写定时任务了，这里我们输入如下代码，然后保存退出。

```
* * * * * echo 123 >> /test.log
```
这个命令的意思是每过一分钟将 123 追加到 /test.log 文件的末尾。

从第一个 `*` 到 最后一个 `*` 分别是 **分、时、日、月、周**。

### 执行 shell 脚本

同时我们也可以编写一个 shell 脚本，让其定时执行。

在 /root 目录下编写 a.sh

```
echo 123 >> /root/a.txt
```

然后利用 `crontab -e` 命令，编辑定时任务：

```
* * * * * sh /root/a.sh >> /root/test.a.log 2>&1
```

上述代码是每分钟执行 a.sh 脚本，并将执行日志写入 /test.a.log 文件。

### 配合 pm2 使用

知道基本用法之后我们配合 pm2 使用，想要每分钟重启id号为0的进程（该进程必须为正在运行中的进程），自然而然就会这样写：

首先编写一个 shell 脚本 /root/b.sh 用于重启id号为0的进程：

```
pm2 restart 0
```

然后利用 crontab 去定时执行这个脚本，并记录其日志，`crontab -e` 输入如下代码（**注意不需要添加 PATH 或者 HOME=/ 等其他东西**，我当时就是看网上添加了 PATH 和 HOME=/ ，之后一直报错）：

```
* * * * * * sh /root/b.sh >> /root/b.log 2>&1
```

然而这样写并不管用，我们打开 /root/b.log 看一下它的报错。

提示 pm2 not found，然后我们使用 `which pm2` 查看 pm2 的路径，将 pm2 换成其路径，重新编辑 `/root/b.shell`：

```
/usr/local/bin/pm2 restart 0
```

这样我们就实现了利用 crontab 每分钟重启 pm2 的某个进程。当然需要根据自己的需求去设定重启时间。

