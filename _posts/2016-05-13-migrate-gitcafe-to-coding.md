---
layout: post
title: "Goodbye to Gitcafe"
description: 中国上海 中国上海 Gitcafe倒闭了，中国上海最大Git仓库 Gitcafe倒闭了
categories:
- 蛋疼的事
tags:
- 博客迁移


---

最近997(Monday - Sunday/ 09 AM - 09 PM)的做一个项目，终于熬到了发布日，本想着从PT站点下载些蓝光原盘，周末在家看个片休息休息，结果发现NAS已经挂了几天了，尝试开机未果，拆机清洁了下灰尘后恢复正常(事实证明硬件设备还是需要定期保洁)

期间为了弄清楚NAS是什么时候买的，想在博客里面查下记录，结果发现博客「又」挂了...是的，我印象中目前托管于Gitcafe的国内镜像博客挂了好多次，这次又是什么原因？打开Gitcafe主页一看，原来人家<s>倒闭</s>被coding收购了，原有的服务即将停止...

![image](http://img.alicdn.com/tps/i4/TB118g3JFXXXXcTaXXXljcQOFXX-1046-587.png)

行吧，迁移就迁移吧，还好过程不复杂，一键处理，然后再去Godaddy将域名CNAME指向到新东家，等待着DNS刷新...

---

第二天发现DNS解析更新了，但是博客首页的内容全空了，再回头看了下Coding的Pages说明，原来人家已经更新至Jekyll 3.0版本，而我本地还是Jekyll 2.5.X的版本，当然不兼容啦...

先升级本地开发环境到Jekyll 3.0看看具体是什么错误，然后就陷入了一连串的排错进程中：

* 执行 `gem install jekyll` 并不会给我升级到最新的版本，应该是本地的gem版本太老
* 于是又升级 `gem update`,接着又提醒我原来用的Ruby淘宝镜像已经不支持http，需要改成https协议
* 等我执行`gem sources --add https://ruby.taobao.org/`时又给我报了一个SSL的错误
* Google半天需要升级本地的openssl库，于是`gem upgrade openssl`
* 终于万事大吉了吧？现实是残酷的，系统又提示我本地的Ruby版本过低（1.9.X）配不上Jekyll 尊贵的 3.0
* 执行`rvm install 2.3.1`安装最新的Ruby，下载速度1KB/S...
* `echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db` 更新下Ruby的源到Ruby China的镜像吧...
* 终于愉快的安装了Ruby
* 这下终于可以执行 `gem install jekyll` 体验最新的3.0版本了...

欢天喜地的执行 `jekyll serve`后终于看到博客报错原因了，主要都是根目录下的`_config.yml`:

* plugins 更名为 plugins_dir （插件目录）
* pygments 更名为 highlighter ，值为 rouge/pygments/null 三者之一 （代码高亮）
* 本地安装 `gem install jekyll-paginate` 后，添加 `gems: [jekyll-paginate]` 在 _config.yml文件里面
* 本地安装 `gem install redcarpet`

![image](http://img.alicdn.com/tps/i4/TB14OJkJVXXXXctXFXXxoofGFXX-882-335.png)

解决完这些冲突后，博客本地可以运行了...

最后一步就是更新 `.git/config` 代码提交的Remote

```shell

vi .git/config

[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
[remote "origin"]
    url = https://github.com/besteric/        besteric.github.io.git
    url = git@git.coding.net:besteric/besteric.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[remote "coding"]
    fetch = +refs/heads/*:refs/remotes/origin/*
    url = git@git.coding.net:besteric/besteric.git
[remote "github"]
    fetch = +refs/heads/*:refs/remotes/origin/*
    url = https://github.com/besteric/besteric.github.io.git
[branch "master"]
    remote = origin
    merge = refs/heads/master  
    
[alias]
    publish = !sh -c \"git push coding master:coding-pages && git push github master &&\" 
    
```

搞定收工，又可以愉快的写博客了...