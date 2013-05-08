---
author: admin
comments: true
date: 2012-07-09 09:09:38
layout: post
slug: '%e4%b8%80%e4%b8%aa%e5%83%8f%e7%b4%a0%e5%bc%95%e5%8f%91%e7%9a%84%e8%a1%80%e6%a1%88'
title: 一个像素引发的血案
wordpress_id: 941
categories:
- 蛋疼的事
---

上礼拜修改了淘宝的全网吊顶，结果上线不到三小时又紧急下线了。原因是设计师发现一个浮出层的边框存在一个像素的白点，而且反复强调“这是完全不能接受的Bug”...

![](http://img04.taobaocdn.com/tps/i4/T1pp2sXdVlXXXVDF63-378-300.png)

**故障原因：**

浮出层由上下两个兄弟节点AB构成，当鼠标Hover AB的父节点C时，控制AB的Border出现


> C:hover A{

border:1px solid #eaeaea;

border-bottom:white;

}

C:hover B{

border:1px solid #eaeaea;

margin-top:-1px;

}


**解决方案：**

问题主要出在border-bottom:white; 或者border-bottom:none；也会出现同样的问题，参考了下[Facebook](http://facebook.com)的实现方式是——border-bottom:0;

**分析缘由：**

1 根据[w3schools里面的相关说明](http://www.w3schools.com/css/css_border.asp)，Border默认有三个属性border-width/border-style/border-color。

2 我们日常使用的border:0等同于border-width=0，border:none等同于border-style:none，不同浏览器渲染这两种方式是不尽相同的，[网上有一篇很详细的文章介绍](http://bbs.blueidea.com/thread-2970677-1-1.html)，在此不再复述。

3 通常认为border:0由于字符少，更有利于流量巨大的网站，比如Facebook的全局吊顶就是采用此类方法。

4 非特殊情况，还是强烈推荐使用border:0 none;彻底让边框消失，世界再次美好起来

**参考资料：**

http://stackoverflow.com/questions/2922909/should-i-use-border-none-or-border-0

http://bbs.blueidea.com/thread-2970677-1-1.html
