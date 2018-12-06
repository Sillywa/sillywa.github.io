---
title: Hexo and GitHub Pages 博客搭建
date: 2018-12-05 15:42:22
tags:
- 博客搭建
categories:
- 博客
---
最近没事想着自己来搭建一个博客，在网上看了一些资料发现，`Hexo + GitHub` 是目前比较常用的博客搭建系统，因此就照着网上的教程一步一步，历经一天左右的时间搭建了这个个人博客。

想着用博客来记录自己的学习笔记，希望自己能把写博客这个习惯坚持下来。

ok，接下来就来看看我是怎么一步步搭建这个博客的。

# 基本环境搭建

## 了解 Hexo
> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装前提

在安装`Hexo`之前我们需要知道电脑里有没有下面的应用程序，如果没有，点击安装，具体安装方法就不做介绍了；如果有则直接看下一步。
- [Node.js](https://nodejs.org/en/)
- [Git](https://git-scm.com/)

## 安装 Hexo

以上两个程序安装成功之后，接下来使用 `npm` 安装 `Hexo`，如果 `npm` 安装较慢，可考虑使用淘宝镜像 [cnpm](https://npm.taobao.org/)，安装完 `cnpm` 之后可将下面所有用到 `npm` 的地方换为`cnpm`。
```
    npm install -g hexo-cli
```
输入以下命令检查 `Hexo` 是否安装成功。
```
    hexo --version
```
如果有版本信息则安装 `Hexo` 成功。

# 开始搭建博客

## 初始化
`Hexo`安装完成之后，用以下命令新建一个文件夹并初始化 `Hexo` 所需文件。
```
    hexo init <folder_name>
    cd folder_name
    npm install
```
`hexo init`过程可能会较慢，请耐心等待。

## 运行
以上过程结束之后，用如下命令在本地运行我们的博客。
```
    hexo server
```
` hexo server ` 可以简写为 `hexo s`。

接着我们用浏览器打开 `localhost:4000` 即可看到我们搭建的博客。

# 将博客放入GitHub
博客搭建好之后，我们在 `GitHub` 新建一个仓库，可以命名为 `your_blog_name.github.io` ，以后就可以直接通过`your_blog_name.github.io`访问你的博客了。

**请务必将仓库名设为xxx.github.io xxx为你自定义，否则之后会出现很多问题**

新建好之后，在你的博客目录下，即前面提到的 `folder_name`下，使用如下命令关联`GitHub`仓库。

如果是第一次使用`GitHub`或者是没有配置 `ssh` 可能会要求输入帐号密码 ，最好的解决办法是[配置ssh](https://segmentfault.com/a/1190000002645623)，然后再进行以下操作。
```
    git init
    git remote add origin <远程仓库地址>
```

接着打开主目录（folder_name）下的 `_config.yml`配置文件，找到 `deploy`，进行如下配置：
```
    type: git
    repo: <远程仓库地址>
    branch: master
```
然后安装以下插件：
```
    npm install hexo-deployer-git --save
```
然后执行以下命令生成静态文件：
```
    hexo generate
```
可简写为 `hexo g`

最后将文件上传到`GitHub`：
```
    hexo deploy
```
可简写为 `hexo d`

# 开启Pages服务
`GitHub`上找到我们的仓库，点击右边的`Settings`：
{% asset_img 1.png %}
下滑找到 `GitHub Pages` ，点击 `master branch`，点击 `save`，即可开启`Pages`服务。
{% asset_img 2.png %}

点击`GitHub Pages`旁边给出的链接即可访问你的博客了。



这样你的博客基本上就搭建成功了，下一篇我们介绍{% post_link usehexo 如何配置和使用Hexo %}。

大家也可以参考[Hexo官网](https://hexo.io/zh-cn/)。



