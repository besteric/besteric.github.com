---
author: admin
comments: true
date: 2012-02-10 03:53:00
layout: post
slug: '%e5%ae%89%e8%a3%85%e8%bf%85%e9%9b%b7%e6%8f%92%e4%bb%b6%e7%9a%84%e7%94%a8%e6%88%b7%e8%ae%bf%e9%97%ae%e6%b7%98%e5%ae%9d%e9%a6%96%e9%a1%b5%e6%8f%90%e7%a4%ba%e4%b8%8b%e8%bd%bdphp%e6%96%87%e4%bb%b6'
title: 安装迅雷插件的用户访问淘宝首页提示下载PHP文件
wordpress_id: 889
categories:
- 蛋疼的事
tags:
- bug
- taobao
- xunlei
---

最近有同事在内网反馈每次使用IE8访问淘宝首页都会蹦出迅雷提示下载**subpromotion_activity_new.php**这个文件，尝试Google了下“[淘宝 迅雷 PHP](http://www.google.com.hk/search?hl=zh-CN&safe=strict&q=%E8%BF%85%E9%9B%B7+%E6%B7%98%E5%AE%9D+php&oq=%E8%BF%85%E9%9B%B7+%E6%B7%98%E5%AE%9D+php&aq=f&aqi=&aql=&gs_sm=3&gs_upl=4008l6029l0l6465l19l9l0l0l0l0l888l888l6-1l1l0)”，发现大部分用户报告此问题的时间点，集中在2011年8-10月之间...虽然确认问题是用户安装了迅雷插件导致的，但是我们还是尝试先从自身排查问题，为什么迅雷插件只识别这个PHP文件？

经过仔细比对，淘宝首页只有中间的一个iFrame，通过src的方式引入了这个php文件，而其他<a>标签通过href的方式引入的PHP文件..前者会被迅雷插件识别。

尝试和迅雷同学接触得到的答复是在2011年8月的某个迅雷release版本修改了**监视下载类型**（添加了*.php文件的支持），后续的版本修复了这个问题，所以使用新版的迅雷用户不会遇到此类问题...希望遇到此类问题的同学及时升级至最新版的迅雷

![](http://img04.taobaocdn.com/tps/i4/T1WXCRXdXeXXXXXXXX-634-478.jpg)
