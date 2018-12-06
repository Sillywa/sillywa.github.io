---
title: NexT 高级配置
date: 2018-12-05 21:28:40
tags:
- 博客配置
categories:
- 博客
---

前一篇文章介绍了NexT的基本配置，其主要涉及两个配置文件第一个是主目录下的`_config.yml`，另一个是我们的主题配置文件`thems/next/_config.yml`，接下来我们继续深入。

# 添加社交网址
在`thems/next/_config.yml`查找 `social`，找到如下代码：
``` ruby
# Social Links.
# Usage: `Key: permalink || icon`
# Key is the link label showing to end users.
# Value before `||` delimeter is the target permalink.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, globe icon will be loaded.
social:
  #GitHub: https://github.com/yourname || github
  E-Mail: mailto:yourname@gmail.com || envelope
  #Weibo: https://weibo.com/yourname || weibo
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype
```
去掉 `social` 的注释并将你需要展示的信息网址注释去掉，可以修改名称和网址。

效果如图：
{% asset_img 1.png %}

# 页面底部添加访问量
在`thems/next/_config.yml`查找 `busuanzi`
```ruby
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```
效果如图：
{% asset_img 2.png %}
本地查看访问量可能有误，但是放到线上就没问题了。

# 为文章添加评论与阅读次数
在`leancloud`上面注册帐号，新建一个应用，找到应用对应的`appid`和`appkey`，然后在`thems/next/_config.yml`查找 `valine`，将填入`appid`和`appkey`以下代码中，相应字段设为`true`：
```ruby
valine:
  enable: true # When enable is set to be true, leancloud_visitors is recommended to be closed for the re-initialization problem within different leancloud adk version.
  appid:  # your leancloud application appid
  appid:  # your leancloud application appkey
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
  visitor: true # leancloud-counter-security is not supported for now. 
```

# 为页面添加搜索功能
在`thems/next/_config.yml`查找 `local_search`，并将`enable`设为`true`。
```ruby
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # unescape html strings to the readable one
  unescape: false
```
然后访问[注释提供的网址](https://github.com/theme-next/hexo-generator-searchdb)，按它的步骤操作。

# 文章分享链接
在`thems/next/_config.yml`查找 `needmoreshare`，并将`enable`设为`true`。
```ruby
needmoreshare2:
  enable: true
  postbottom:
    enable: true
    options:
      iconStyle: box
      boxForm: horizontal
      position: bottomCenter
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook
  float:
    enable: true
    options:
      iconStyle: box
      boxForm: horizontal
      position: middleRight
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook

```
然后访问[注释提供的网址](https://github.com/theme-next/theme-next-needmoreshare2)，按它的步骤操作。

# 博客页脚记时
打开 `\themes\next\layout\_partials\footer.swig`，在最下面添加如下代码：
```ruby
<script>
    var now = new Date();
    function createtime() {
        var grt= new Date("12/03/2018 00:00:00");//此处修改你的建站时间或者网站上线时间
        now.setTime(now.getTime()+250);
        days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days);
        hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours);
        if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
        mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;}
        seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
        snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;}
        document.getElementById("timeDate").innerHTML = "Running for "+dnum+" Days ";
        document.getElementById("times").innerHTML = hnum + " Hours " + mnum + " m " + snum + " s";
    }
    setInterval("createtime()",250);
</script>
```
并将以下代码放在这个文件你喜欢的位置，然后查看效果。
```ruby
<div>
<span id="timeDate"></span><span id="times"></span>
</div>
```

`NexT`高级配置就介绍这么多，至此，我们已搭建出一个比较完整的博客，然后接下来就可以快乐的写博客啦！