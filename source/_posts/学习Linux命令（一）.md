---
title: 学习Linux命令
date: 2018-12-06 13:34:03
tags:
- Linux
categories:
- 操作系统
---
# Linux系统
* `pwd` 打印当前工作目录
* `cd` 改变目录
```
    cd /usr/bin  绝对路径从根目录出发，到达目标目录
    cd ./usr 相对路径从工作目录出发，到达目标目录
    cd .. 到达父目录
    cd 到达主目录
```
* `ls` 列出目录内容
```
    ls -l 使用长格式显示结果
    ls -t 按修改时间排序
    ls -r 以相反的顺序显示
    ls -S 按文件大小对结果进行排序
    ......
```
* `file` 确定文件类型
```
    file filename
```
* `less` 查看文件内容
```
    less /etc/passwd
```

# 操作文件与目录
* `mkdir` 创建目录
```
    mkdir dir1              创建单个目录
    mkdir dir1 dir2 dir3    创建多个目录
```
* `cp` 复制文件或目录
```
    cp file1 file2          将文件file1复制到file2中,file2内容将会被覆盖
    cp -r dir1 dir2         复制目录时一定要加 -r
    cp file1 file2 dir1     将多个文件复制到一个目录下
```
`cp`命令选项
```
    -i          在覆盖一个已存在的文件前，提示用户进行确认。
    -r          递归复制目录及其内容。复制目录时需要这个选项
    -u          将文件从一个目录复制到另一个目录时，只会复制目标目录不存在的文件或是目标目录相应文件的更新文件
    -v          复制文件时显示信息性消息
```
* `mv` 重命名或移动文件和目录
```
    mv item1 item2              将文件或目录item1移动或重命名为item2
    mv item1 item2 item3 dir1   将多个条目移动到dir1目录下
```
`mv`命令选项与`cp`大致相同，`mv`没有`-r`选项
```
    -i          在覆盖一个已存在的文件前，提示用户进行确认。
    -u          将文件从一个目录移动到另一个目录时，只会移动目标目录不存在的文件或是目标目录相应文件的更新文件
    -v          移动时显示信息性消息
```
* `rm` 删除文件或目录
```
    rm -r item1 item2 item3         删除item1,item2,item3,删除目录时需要-r
    rm *.html                       删除以.html结尾的文件
```
`rm`命令选项
```
    -i          删除前提示用户确认
    -r          递归删除目录及其内容。删除目录时需要这个选项
    -f          忽略不存在的文件，并无需提示确认
    -v          删除时显示信息性消息
```
* `ln` 创建硬链接和符号链接
```
    ln file hard-link-name      创建file文件的硬链接
    ln -s file sym-link-name    创建file文件的符号链接，符号链接指向源文件，与源文件内容保持一致
```
`file`为相对于`sym-link-name`的文件，即为相对路径，当然也可以是绝对路径
```
    ln -s ../file sym-link-name     file在当前目录的父目录中，即file相对于sym-link-name的位置
```