---
title: Linux下阿里云镜像安装以及Nginx安装
date: 2019-02-27 19:20:15
top: 10
tags:
- Linux
categories:
- 操作系统
---
本篇文章主要记录了`CentOS 7`系统以及`RedHat 7`系统如何安装阿里云镜像以及`Nginx`，并对`Nginx`实现基本配置。

目前`RHEL/CentOS`软件包主要有三种类型：
- `RPM`包
  `rpm`是一个完整的数据库平台，包含软件包的版本、安装路径、配置文件等全方面的服务，提供的查询、安装、卸载、升级四大功能，尤其是查询功能，常用的命令有：

  ```
    -q    查询指定软件包是否安装
    -qa   查询所有已安装软件列表
    -qi   查询指定软件包信息
    -ql   查询指定软件包文件列表
    -qc   查询指定软件包配置文件
    -qf   根据文件路径反向查找软件包
  ```
  由于`rpm`各个包的依赖性太强，因此一般通过`yum`进行`rpm`包的批量安装，类似于前端的npm包文管理工具

- 源码包
  源码包更新及时，可定制化，但是需要自己手动编译，其依赖于编译环境，不推荐新手使用

- 绿色包

其中我们比较常用的就是通过`yum`进行`rpm`包的安装。

`yum`仓库早期使用系统安装光盘，或者系统自带，更新不及时，安装速度也比较慢。

随着技术的发展，目前国内有些比较好的镜像源：如[阿里云](https://opsx.alibaba.com/mirror)、[163](https://mirrors.163.com)，都可以提供比较便捷的`yum`仓库服务。因此我们需要使用国内镜像来通过`yum`来进行`RPM`包的安装。

对于`CentOS`系统而言，官方自带`yum`库，而`RHEL`不提供`yum`库，因此在安装镜像之前`RHEL`需要卸载原来的红帽`yum`源，装上`CentOS`的`yum`组件。

接下来就是具体的安装步骤。

## CentOS
由于`CentOS 7`自带官方`yum`库，因此可以直接安装阿里云镜像：
```
1、备份

mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

2、下载新的CentOS-Base.repo 到/etc/yum.repos.d/

CentOS 5

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
或者

curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
CentOS 6

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
或者

curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
CentOS 7

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
或者

curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

3、之后运行yum makecache生成缓存


```
也可以参看[阿里云镜像站](https://opsx.alibaba.com/mirror)。

## REHEL 7
`redhat`相对`CentOS`系统麻烦一些，具体步骤：

1. 卸载红帽`yum`源
```
rpm -e $(rpm -qa|grep yum) --nodeps
```

2. 删除所有`repo`文件
```
rm -rf /etc/yum.conf
rm -rf /etc/yum.repos.d/
rm -rf /var/cache/yum
```

3. 下载`CentOS`相关的`yum`组件
```
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-3.4.3-161.el7.centos.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-updateonboot-1.1.31-50.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-utils-1.1.31-50.el7.noarch.rpm


//如果没有wget命令则使用curl命令
curl -o yum-utils-1.1.31-50.el7.noarch.rpm https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-utils-1.1.31-50.el7.noarch.rpm
curl -o yum-3.4.3-161.el7.centos.noarch.rpm https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-3.4.3-161.el7.centos.noarch.rpm
curl -o yum-metadata-parser-1.1.4-10.el7.x86_64.rpm https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
curl -o yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm
curl -o yum-updateonboot-1.1.31-50.el7.noarch.rpm https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-updateonboot-1.1.31-50.el7.noarch.rpm
```
安装时需要注意各个组件是否为最新版本，可前往目标网址查看。

4. 安装所有相关组件
```
rpm -ivh yum-* --nodeps
```

5. 下载阿里云`base`仓库
```
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sed -i 's#\$releasever#7#g' /etc/yum.repos.d/CentOS-Base.repo

```

## Nginx安装

经过以上步骤，我们已经安装了`yum`的阿里云镜像，里面只包含一些基本的库，因此我们还需要安装阿里云`epel`库。
```
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

接着可以下载`Nginx`官方镜像源。
```
vim /etc/yum.repos.d/nginx.repo
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
```

**需要特别注意的是：`$releasever`为你系统的版本号**。
{% asset_img 1.png [注意] %}

然后就可以执行安装`Nginx`了：
```
yum install -y nginx
```
启动`Nginx`：
```
systemctl start nginx   临时开启
systemctl enable nginx  永久开启
```

浏览器输入ip地址即可访问，如访问不了，请关闭防火墙。
```
systemctl stop firewalld      临时关闭
systemctl disable firewalld   永久关闭
```

## Nginx文件说明
全局配置文件：`/etc/nginx/nginx.conf`

局部配置文件：`/etc/nginx/conf.d/*.conf`

日志文件：`/var/log/nginx/{access.log error.log}`
	访问日志：`access.log`
	错误日志：`error.log`

文档根目录：`/usr/share/nginx/html`	

如果要通过域名访问虚拟机，需要修改本地主机名解析记录：
1. `vim /etc/hosts`
2. 在文件中加入 `虚拟机ip 域名` 即可
3. 访问本机 `c:\Windows\System32\drivers\etc\hosts`
4. 修改`hosts`文件，同第二步一样

有关`Nginx`的更多可以参考{% post_link nginx-1 Ubuntu 系统 Nginx 服务下 ssl 证书配置 %} 以及 {% post_link nginx-2 Nginx 网站配置以及 NodeJS API 配置 %}
