---
author: admin
comments: true
date: 2010-01-22 16:33:07
layout: post
slug: '%e5%a6%82%e4%bd%95%e8%ae%a9selenium%e6%94%af%e6%8c%81%e6%9c%80%e6%96%b0%e7%89%88%e6%9c%ac%e7%9a%84firefox-3-6'
title: 如何让Selenium支持最新版本的Firefox 3.6
wordpress_id: 373
categories:
- Selenium
- 蛋疼的事
tags:
- Selenium
---

其实主要让Selenium不支持Firefox最新版的罪魁祸首就是每次运行测试的时候都会加载一些 plugin进去，而FF的插件基本都有严格的版本限制规则，别人国外做事比较严谨，所以在这个地方写的比较死，呵呵，下图为Selenium运行测试时候加载的纯净版Firefox的插件列表：

[![](http://www.besteric.com/wp-content/uploads/2010/01/2010-1-22-16-13-47-300x236.png)](http://www.besteric.com/wp-content/uploads/2010/01/2010-1-22-16-13-47.png)

解决办法：

1、用winrar打开**selenium-server.jar**；
2、查找两个目录：**customProfileDirCUSTFFCHROME**和**customProfileDirCUSTFF**；
3、搜索每个目录，直到找到文件**install.rdf**，**解压缩到一个临时目录**，编辑如下行：

`

       
            
                {ec8030f7-c20a-464f-9b0e-13a3a9e97384}
                1.4.1
                3.5.*
           
         
`

To

`
       
            
                {ec8030f7-c20a-464f-9b0e-13a3a9e97384}
                1.4.1
                3.6.*
           
         
`

当然你乐意改成4.0.*我也木有意见，未雨绸缪么，呵呵


