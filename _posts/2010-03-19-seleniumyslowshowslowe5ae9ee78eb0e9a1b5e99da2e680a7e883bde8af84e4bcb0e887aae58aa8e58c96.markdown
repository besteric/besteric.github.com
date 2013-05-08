---
author: admin
comments: true
date: 2010-03-19 14:42:44
layout: post
slug: seleniumyslowshowslow%e5%ae%9e%e7%8e%b0%e9%a1%b5%e9%9d%a2%e6%80%a7%e8%83%bd%e8%af%84%e4%bc%b0%e8%87%aa%e5%8a%a8%e5%8c%96
title: Selenium+YSlow+ShowSlow实现页面性能评估自动化
wordpress_id: 479
categories:
- 蛋疼的事
tags:
- Selenium
- ShowSlow
- YSlow
- 性能测试
---

**工具介绍**

**[Firebug](http://getfirebug.com/): **这个不介绍了，居家旅行杀人越货必备的Firefox插件

**[YSlow](http://developer.yahoo.com/yslow/)**: 当Firefox浏览网页时，可以分析网站的页面（基于Yahoo 14条评分原则），并告诉你为了提高网站性能，如何基于某些规则而进行优化

**[ShowSlow](http://www.showslow.com/):收集YSlow的测试结果并显示出来**

**思路整理**

使用Selenium编写测试用例依次访问需要测试的站点，设置YSlow每打开一个页面自动运行测试，并将测试报告发送到指定的ShowSlow服务器，建议可以和我前几天说的自动化框架搭配使用，何乐而不为:)

**环境配置**

**搭建本地ShowSlow平台**

默认情况下YSlow的结果会发送到ShowSlow（印象中），但这显然不符合天朝国情，同时也及其不和谐，还好ShowSlow开源网站提供源码可以在本地搭建一个平台来收集YSlow的信息。主要采用**Apache+PHP+Mysql**这个框架，但是很不幸的告诉各位我尝试过独自手工搭建上述环境，弄坏了两台虚拟机（Win 2003）都未遂，主要在于没有相关经验，在此推荐使用 **AppServ**傻瓜化一体式安装吧（请尽情鄙视我，谢谢）

1.先用SVN将源码下载到本地，并放置于Apache的WWW文件夹下（[请猛击我](http://code.google.com/p/showslow/source/checkout)）

2.修改ShowSlow文件夹下的**config.sample.php**重命名为**config.php**,里面**$db,$user,$pass**可以根据实际情况修改

3.创建第2步中你填写的数据库，MySQL我也不太会，高手无视我




> 

> 
> //创建一个数据库，名字和第二步你填写的保持一致
> 
> 

> 
> **create database '****DBName****';**
> 
> 

> 
> //切换到新建的数据库
> 
> 

> 
> **use 'DBName';**
> 
> 

> 
> // 将ShowSlow文件夹的tables.sql（数据库表）导入到新建的数据库中，注意无分号
> 
> 

> 
> **source c:\tables.sql**
> 
> 

> 
> //查看下是否导入成功了，貌似有个表名叫ShowSlow2，汗
> 
> 

> 
> **show tables;**


**配置YSlow**

1.打开Firefox输入：about:config（我保证会很小心的）

2.filter中输入：yslow

3.修改以下三条数据


> extensions.yslow.beaconUrl = **http://localhost/showslow/beacon/yslow/**

**如果测试和服务器不在同一机器上，请将localhost改成实际地址**

**extensions.yslow.beaconInfo = **grade****

****extensions.yslow.optinBeacon = **true******


4.重启Firefox，have fun :)

还等什么？开始你的测试之旅吧，查看测试报告的URL是：**http://localhost/showslow/**

**感谢51testing斑竹小米啊提供的解决方案（[围观请猛击我](http://bbs.51testing.com/viewthread.php?tid=178641)）**

**PS：Google的Page Speed和YSlow差不多，也支持发送报告至ShowSlow，有兴趣的同学可以试试**
