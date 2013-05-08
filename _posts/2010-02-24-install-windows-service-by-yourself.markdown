---
author: admin
comments: true
date: 2010-02-24 14:39:55
layout: post
slug: '%e6%89%8b%e5%8a%a8%e5%ae%89%e8%a3%85windows-service'
title: 手动安装Windows Service
wordpress_id: 422
categories:
- 蛋疼的事
---

sc create "Eric" binPath= "c:\windows\Eric.exe" displayname= "Eric" start= auto
注意=后面的空格不能省略

如果路径中有空格....则
sc create "Eric" binPath= "\"c:\program files\Eric.exe\" agv1 agv2" displayname= "Eric" start= auto
