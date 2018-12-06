---
title: Hexo 及 NexT 基本配置与使用
date: 2018-12-05 17:46:51
tags:
- 博客配置
categories:
- 博客
---

前一篇文章介绍了如何搭建博客，但是没有介绍如何使用和个性化配置博客。因此这篇文章主要来介绍`Hexo`的主题及其配置以及如何来写自己的博客。

# 主题下载与应用
`Hexo`提供各种各样的主题，我们可以进入[官网](https://hexo.io/themes/)去选择自己喜欢的主题，然后在`GitHub`上有其具体的介绍。

接下来我们以[NexT](https://github.com/theme-next/hexo-theme-next)主题为例进行介绍。

截至目前为止，`NexT`主题已经从v5.1.x更新至[v6.6.0](https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/UPDATE-FROM-5.1.X.md)，仓库也从原来的老仓库迁移到[这里](https://github.com/theme-next/hexo-theme-next)。因此`NexT`主题的很多配置都和以前不一样了，我当时在网上看的时候全是老版本的配置方法，花费了不少时间。最后发现其实可以自己看着`themes`下的`_config.yml`进行配置，很多插件都在[theme-next](https://github.com/theme-next)这个仓库有。

## 下载 NexT
切换到主目录，然后克隆整个仓库到`themes/next`：
```
    cd hexo
    git clone https://github.com/theme-next/hexo-theme-next themes/next
```
之后我们会发现 `themes`下多了个`next`文件夹，即我们的主题文件夹。

## 配置
整个 `Hexo` 博客有两个主要的配置文件，第一个是主目录下的`_config.yml`，另一个是我们的主题配置文件，是`thems/next/_config.yml`。

现在我们开始将我们下载的主题应用到我们的博客中，我们只需修改主目录下的`_config.yml`，如下：
```ruby
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/

theme: next
```
然后`hexo s`启动博客即可。
**需要注意的是：每当我们修改了主目录下的`_config.yml`，只有重启博客服务才能生效；而修改`thems/next/_config.yml`是不需要重启博客服务的。**

同样我们可以在主目录下的`_config.yml`进行其他设置，我们可以看到里面有网站基本设置，如下：
```
title: Hexo           
subtitle:
description:
keywords:
author: John Doe
language:
timezone:
```
| 参数  | 描述 |
| ---- | ---- |
| title | 网站标题 |
| subtitle | 网站副标题 |
| description | 网站描述 |
| author | 作者名字 |
| language | 网站语言，NexT v6.0.3以后中文设为 zh-CN |

具体全部配置参考[官方文档](https://hexo.io/zh-cn/docs/configuration)。

我们暂时不需要全部理解其意思，只要把网站的基本描述改为你自己的就好。

# 主题设定

## 选择 Scheme
`Scheme` 的切换通过更改主题配置文件，打开`thems/next/_config.yml`，搜索 scheme 关键字。 你会看到有三行 `scheme` 的配置，将你需用启用的 `scheme` 前面注释 `#` 去除即可。
```ruby
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```
> Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。
> Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
> Mist - Muse 的紧凑版本，整洁有序的单栏外观
> Pisces - 双栏 Scheme，小家碧玉似的清新

选择对应的外观，刷新浏览器即可预览。

## 设置菜单

打开`thems/next/_config.yml`，找到如下代码
```ruby
menu:
  home: / || home
  #about: /about/ || user
  #tags: /tags/ || tags
  #categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```
这里是进行菜单配置，去掉哪个注释，就会多一个相应的菜单选项。

当需要`about`、`tags`、`categories` 需要手动创建这个页面，如果不创建点击则不会出现相应页面。

使用如下命令创建这些文件夹

```
hexo new page "about"
hexo new page "tags"
hexo new page "categories"
```
之后`source`文件夹下就会出现三个这样的文件夹。

## 设置头像
打开`thems/next/_config.yml`，找到如下代码
```ruby
avatar:
  # in theme directory(source/images): /images/avatar.gif
  # in site  directory(source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: #/images/avatar.gif
  # If true, the avatar would be dispalyed in circle.
  rounded: false
  # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
  opacity: 1
  # If true, the avatar would be rotated with the cursor.
  rotated: false
```
修改字段 `avatar`， 值设置成头像的链接地址，参考[这个链接](http://theme-next.iissnan.com/getting-started.html#avatar-setting)。

以上主题设置可以参考[Next文档](http://theme-next.iissnan.com/)。

## tags 和 categories 设置

当菜单中有了 `tags` 和 `categories` 时，我们需要在 `Front-matter` 中添加 `type` 属性。所谓 [Front-matter](https://hexo.io/zh-cn/docs/front-matter) 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量。

`tags/index.md`
```
---
title: 标签
date: 2018-12-05 10:00:29
type: "tags"
---

```
`categories/index.md` 同理。

只有这样当我们新建一篇博客时，指定的`tags`和`categories`才会同步，`hexo`才会识别出来你的 `tags` 和 `categories`。所以接下来我们看如何新建一篇博客。


# 新建博客
新建博客很简单，使用如下命令
```
hexo new "文章题目"
```
这样就会在`source`目录下自动创建一个名为 `文章题目.md` 的文件，我们只要在这个文件上写文章就行了。同样我们需要每篇文章指定一个或多个 `tags` 和 一个 `categories`。这样你的菜单中`tags` 页面 和`categories`页面就会有内容了。
```
---
title: 文章题目
date: 2018-12-05 15:42:22
tags:
- PS3
- Games
categories:
- Diary
---

```


这样整个 `NexT` 基本配置就结束了，之后将会介绍一些{% post_link topnext 更高级的配置 %}。


