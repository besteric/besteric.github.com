---
layout: post
title: "国内的 Git Pages 服务没一个能打的"
description: Coding.net 的 Pages 服务器在香港，延迟高达 2000ms; Gitee Pages 想使用个性域名需要付费 99/年
categories:
- 蛋疼的事
tags:
- 


---

### 前言

很早之前感慨 Github 国内访问偶有不顺，且国内访问的延迟较高，考虑到我的博客大部分受众还是居住在墙内为主，所以尝试将代码同步至国内的几个 Git 托管商（镜像），并使用他们的 Pages 服务对外输出，从最早的 Gitcafe 到它被收购后的 Coding，感觉体验还不错，但是自从 Coding 被腾讯入股（收购?）后，Pages 服务就不那么稳定了，最近一段时间延迟高达 2000ms（官方说是经常被 DDNS），病急乱投医的又跑去注册了一个 Gitee 的账号，赫然发现它的 Pages 服务居然还要付费 99/年 才能使用个性域名...

思前想后，还是回归 Github Pages 大本营吧，国内各种不靠谱，不和你们玩啦

### 修改 DNS 解析

前段时间域名到期，想去 GoDaddy 续费，尝试支付宝渠道多次未果，想想它家服务也确实一般，一气之下直接将域名迁至 Name.com 服务商，立马药到病除，全身都舒坦了...

这次也是很简单的将 DNS 解析新增了 2 条 CNAME

```
from besteric.com to besteric.github.io
from www.besteric.com to besteric.github.io
```

### 解决 SSL 证书问题

由于我的博客一直都是多个源同时更新，所以 Github 本身就是最新的代码，DNS 切换后没多久就立马生效了，但是马上跳出一个问题，就是 Chrome 浏览器会提示我的网址是个「不安全的地址」，罪魁祸首就是 Github 并不对个性域名颁发有效的 SSL 证书

解决方案也很简单，使用境外 CDN 加速服务 cloudflare 或者使用国内的阿里云服务（免费1年），懒癌患者当然选择一步到位，义不容辞的选择了 cloudflare 服务...

问题马上又来了，cloudflare 我很早就使用过它家的服务，这次将 besteric.com 添加后，系统提示 Code: 1097，也就是说我的域名被 Banned 了...WTF... Google 了下社区的反馈，只能发邮件给官方询问具体的封禁原因，等了3天，官方简短的回复如下：

```
Hello,

We have lifted the restrictions against the domain and it may now be added to Cloudflare.
```

好吧，解禁就行了吧，我也懒得追问为什么把我的域名给 Banned，根据提示将 name.com 的域名服务器修改为 lola.ns.cloudflare.com 和 noah.ns.cloudflare.com 两条记录，保存后，稍等片刻（我大概等了1小时）生效，就可以正常访问博客了

最后在申请免费的 SSL 证书，跟随[爲GitHub Pages配置Cloudflare的免費SSL數字證書](https://axdlog.com/zh/2018/using-cloudflare-free-ssl-in-github-pages-with-custom-domain/)的步骤即可

### 默认使用 HTTPS 访问

回到 Github Pages 的设置界面，记得勾选最后一项的 ```Enforce HTTPS``` 选项，这样即使用户使用 http 访问也会强制跳转至 https 协议访问你的域名（很黄很暴力，我喜欢）

最后别忘记修改 Jekyll 的 config 把所有的访问地址都修改为 https

OK，大功告成，静静的享受一个被国外最大基佬网站托管的静态文件服务，并且使用了免费的 SSL 证书的个人网站吧，Enjoy it

--EOF--