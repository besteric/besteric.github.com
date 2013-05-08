---
author: admin
comments: true
date: 2010-01-13 14:45:51
layout: post
slug: '%e8%a7%a3%e5%86%b3selenium%e4%bd%bf%e7%94%a8firefox%e6%b5%8b%e8%af%95ssl%e9%a1%b5%e9%9d%a2%e5%8f%8d%e5%a4%8d%e6%8f%90%e7%a4%ba%e4%b8%8d%e4%bf%a1%e4%bb%bb%e9%97%ae%e9%a2%98'
title: 解决Selenium使用Firefox测试SSL页面反复提示不信任问题
wordpress_id: 333
categories:
- Selenium
tags:
- Selenium
- SSL
---

以前就遇到过使用Firefox测试SSL站点显示不信任站点的问题 “The Connection is Untrusted”, 可恼的是Selenium每次运行Test case都需要手动添加信任站点，非常不和谐，在Selenium 的Google Group 搜了下 “Untrusted” 发现解决方案：

[![](http://www.besteric.com/wp-content/uploads/2010/01/2010-1-13-14-04-06.jpg)](http://www.besteric.com/wp-content/uploads/2010/01/2010-1-13-14-04-06.jpg)

1.对Firefox建立两个Profile——CMD命令窗口 执行 firefox -ProfileManager （当然你的Environment Path 设置好了FF的路径）

2.删除你这个Profile的所有文件，除了cert_override 和 cert8

3.启动Selenium Server 使用如下命令 ：java -jar selenium-server.jar -firefoxProfileTemplate c:\.....(directory)文件名最好别有空格

华丽丽的解决了一个小问题

今天发现有个更简单的办法：Remember Certificate Exception 这个firefox插件，只要弹出证书窗口，全都自动搞定。

2010/1/20更新：

我们每次运行selenium测试的时候默认都是启用一个“纯净”的Firefox Profiles设置，这样如果需要测试中使用一些插件，比如上述的Remember Certificate Exception以及Firebug ，IDE之类的就必须使用第一种办法调用一个固定的Profiles。
