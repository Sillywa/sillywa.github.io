---
title: Nginx 网站配置以及 NodeJS API 配置
date: 2018-12-23 21:26:36
top: 10
tags:
- Nginx
categories:
- Linux
---

### 网站配置简单说明

`Nginx` 主配置文件为  `/etc/nginx/nginx.conf`

{%  asset_img 2.png %}

`Nginx` 的 `server`模块配置文件放在 `/etc/nginx/sites-available`目录，该目录下默认有一个 `default` 文件，该文件为 `server` 模块文件。

{%  asset_img 1.png %}

我们可以看到 `root` 后面的路径就是我们网站存放的位置，因此你可以根据实际情况自己修改，我的网站是放在 `/var/www/sillywa.blog` 目录下，`Nginx` 会自动寻找该目录下的 `index.html` 文件。

其中 `server_name` 后面可以放我们的域名，多个域名用空格隔开

我们可以自己在 `default` 文件中新建其他 `server` 模块，`nginx.conf` 的 `http` 模块默认包含该目录下所有的文件

{%  asset_img 3.png %}

不过通过上图我们发现`nginx.conf` 默认包含的是 `/etc/nginx/sites-enabled/*` 下的所有文件，但是我们发现该目录下有一个 `default` 软链接，该软链接指向`/etc/nginx/sites-available/default` 文件，因此，对`/etc/nginx/sites-available/default` 文件的修改会同步到 `/etc/nginx/sites-enabled/default` 

{%  asset_img 4.png %}

当然，除了直接修改  `/etc/nginx/sites-available/default` 文件外，我们也可以在 `/etc/nginx/conf.d` 文件夹下自己添加 `server` 配置文件，文件以`.conf`结尾。

### Nginx 与 NodeJS 简单结合

在 `Nginx` 中设置一个代理，让所有请求跳转到 `NodeJS` 服务的接口。

这是我们写好的 `NodeJS` 代码：

{%  asset_img 5.png %}

在 `NodeJS` 中我们设置了允许跨域，同时提供一个 `post` 方法的接口， `NodeJS` 监听 8888 端口。于是，我们在 `Nginx` 中加入如下配置：

可以在 `/etc/nginx/conf.d`文件夹下新建一个 `api.conf` 文件，然后写入如下配置：

{%  asset_img 6.png %}

其中的 `server_name` 是前端请求的 `api` 地址， `ssl` 是我们配置的 `ssl` 证书，可以参考{% post_link nginx-1 上一篇文章 %}，`location` 做一个跳转，当有请求发到 `https://api.sillywa.com` 的时候， `Nginx` 会将请求转发到 `http://localhost:8888`，这样 `NodeJS` 就能接收到请求。

由于我们在 `NodeJS` 中允许了跨域，因此可以不必在 `Nginx` 中进行其他设置。

如果我们没有在 `NodeJS` 中做跨域， `Nginx` 中可以增加如下配置：

{%  asset_img 7.png %}




