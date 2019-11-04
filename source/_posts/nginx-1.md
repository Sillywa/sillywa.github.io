---
title: Nginx 服务下 ssl 证书配置
date: 2018-12-22 16:34:05
top: 10
tags:
- Nginx
categories:
- Linux
---
最近在折腾 `Ubuntu` 系统以及如何让网站可以 `https` 访问，于是就了解到 `ssl` 证书以及 `Nginx` 服务。通过配置 `Nginx` 服务就可以让我们的网站可以通过 `https` 访问了。当然除了 `Nginx` 服务器可以选择之外，我们也可以利用 `Apache、Tomcat、IIS`等其他服务器，本文主要介绍 `Nginx。`

### 安装 Nginx

```
sudo apt-get install nginx
// 查看nginx版本
nginx -v
```
`Ubuntu` 安装 `Nginx` 之后的文件结构大致为：

- 所有配置文件都在 `/etc/nginx` 下面，并且每个虚拟主机已经安排在了 `/etc/nginx/sites-available` 下，该文件夹下有一个 `default` 配置文件
- 程序放在了 `/usr/sbin/nginx` 
- 日志放在了 `/var/log/nginx` 中
- 启动脚本放在 `/etc/init.d/` 下
- 默认的虚拟主机的目录设置在了 `/var/www/nginx-default` (或者是 `/var/www`)，也就是说你的网站可以放在这个目录下

### 启动 Nginx

```
sudo /etc/init.d/nginx start
```

之后可以访问 `http://你的公网 ip` ，默认监听 80 端口，启动时候若显示端口 80 被占用：` Starting nginx: [emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)` 修改文件：`/etc/nginx/sites-available/default`,去掉 `listen` 前面的 # 号 , # 号在该文件里是注释的意思 , 并且把 `listen` 后面的 80 端口号改为自己的端口，访问时需要添加端口号。

### 配置 ssl 证书

阿里云购买的域名可以申请免费的 `ssl` 证书。下载之后就会有两个文件，一个 `.key` 文件，一个 `.pem `文件，`.key` 文件是证书私钥文件，`.pem` 文件是证书文件，一般包含两段内容。一般 `Nginx` 的一些文档会用该扩展名文件，在阿里云证书中与`.crt`文件一样。

最终我们获得了两个文件：

```
example_com.key
example_com.pem
```

为了统一位置，我们可以把这两个文件放在 `/etc/ssl/private/` 目录，然后进入 `/etc/nginx/sites-available/` 目录修改 `default` 文件

```
server {  
    listen 80 default_server;
    listen [::]:80 default_server; 
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;
    server_name example.com;

    ssl on;
    ssl_certificate /etc/ssl/private/example_com.pem;
    ssl_certificate_key /etc/ssl/private/example_com.key;
}
```

配置完成之后重启 `Nginx` 服务：

```
sudo /etc/init.d/nginx reload
```

之后就可以用 `https://www.example.com` 访问你的网站了。如果是第一次配置，访问到的就应该是 `Nginx` 的欢迎页面，该页面一般存放在我们前面说的  `/var/www/nginx-default` 文件夹下，这里的  `nginx-default` 一般为 `html` 文件夹，该文件夹下有一个 `.html` 文件。






