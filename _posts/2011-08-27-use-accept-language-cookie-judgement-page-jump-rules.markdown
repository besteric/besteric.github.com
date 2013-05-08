---
author: admin
comments: true
date: 2011-08-27 14:18:40
layout: post
slug: '%e4%bd%bf%e7%94%a8accept-languagecookie%e5%88%a4%e6%96%ad%e9%a1%b5%e9%9d%a2%e8%b7%b3%e8%bd%ac%e9%80%bb%e8%be%91%ef%bc%88%e6%b7%98%e5%ae%9d%e9%a6%96%e9%a1%b5%e6%b5%b7%e5%a4%96%e7%89%88%e5%ae%9e'
title: 使用Accept-Language+Cookie判断页面跳转逻辑
wordpress_id: 841
categories:
- 蛋疼的事
---



Accept-Language
用户可以通过设置浏览器的语言来控制该值，中文繁体的值有：zh-hk/zh-tw/zh-mo/zh-hant；中文简体的值有：zh-cn/zh-sg/zh-hans；其中zh-hant/zh-CHT 和zh-hans/zh-CHS 只在IE浏览器下出现。

简单的介绍一下其格式：zh-hk中，’-’前面的两个字符表示语言的种类，后面的那个字符表示区域，此值是不分大小写的。一般，用户可能同时设置多种语言环境，这样，在浏览器发起请求的时候，accept-language会带上这所有的语言类型，但每一个语言类型都会有一个权值大小。例如：Accept-Language: zh-CN,zh-HK;q=0.7,zh-SG;q=0.3

这里，我设置了多种语言环境，最后表现为，不同的语言种类会以‘,’隔开。而备选的语言环境后面，都会有一个权值，即q值，默认为1。我们可根据q值大小来决定使用哪一个语言环境。 设置选项中排在第一位的权值为1，而且在accept-language里面出现在第一位，后面的次之。使用accept-language的好处是，可以根据用户的浏览器语言环境来判断用户所希望出现的页面。但是，让用户去改浏览器语言，会稍微有点麻烦，而且改掉浏览器语言，会影响到所有的网页，这可能不是用户想要的效果。

Cookie
可以使用cookie来记录用户的语言喜好，好处是用户可以设置当前网站的语言环境，而不会影响到其它网站，坏处是，如果没有设置cookie，则无法判断用户的语言环境。

简单的解决办法就是利用Accept-Language+Cookie综合判断

![](http://www.besteric.com/wp-content/uploads/2011/08/al.jpg)

让我们看看Facebook的实际效果吧，首先设置下Chrome的首选语言为日语

![](http://www.besteric.com/wp-content/uploads/2011/08/image008.jpg)

访问Facebook，首页默认显示日语，手工选择右下的中文（简体）后再次访问就会自动跳转中文页面

![](http://www.besteric.com/wp-content/uploads/2011/08/image010.jpg)

查询Facebook的Cookie可以看到增加了一个locale=zh_CN的字段

![](http://www.besteric.com/wp-content/uploads/2011/08/help.png)

回过头来看看淘宝首页的逻辑判断需要在Nginx服务器上写一个简单的判断

![](http://www.besteric.com/wp-content/uploads/2011/08/image002.jpg)

OK，大功告成...这个配置目前针对港澳台的服务器生效，如果想看看实际效果欢迎绑定以下Hosts访问


119.167.201.241  www.taobao.com



