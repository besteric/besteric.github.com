---
author: admin
comments: true
date: 2010-04-28 14:55:06
layout: post
slug: xvfbyslowshowslow%e6%90%ad%e5%bb%ba%e5%89%8d%e7%ab%af%e6%80%a7%e8%83%bd%e6%b5%8b%e8%af%95%e6%a1%86%e6%9e%b6
title: Xvfb+YSlow+ShowSlow搭建前端性能测试框架
wordpress_id: 533
categories:
- Performance
tags:
- ShowSlow
- Xvfb
- YSlow
---

**工具介绍**

[Xvfb](http://en.wikipedia.org/wiki/Xvfb):  主要就是通过内存计算模拟出图形界面，没有平常所见的操作界面，分为客户端和服务器

**[YSlow](http://developer.yahoo.com/yslow/)**: 当Firefox浏览网页时，可以分析网站的页面（基于Yahoo 14条评分原则），并指出如何进行优化提高网站性能

**[ShowSlow](http://www.showslow.com/):收集YSlow的测试结果并显示出来**

[Ubuntu](http://www.ubuntu.com/)：开源的Linux系统，界面越来越向Windows靠近

**框架简介**

对于前端的童鞋我相信YSlow已经耳熟能详了，基于雅虎的评分规则对页面进行评分的Firefox插件，从中我们可以看出我们页面上的很多不足，并且可以知道我们如何改进和优化，配合将测试报告发送到本地的ShowSlow平台以提供给开发人员随时查看。在Xvfb的辅助下，**这个框架最大的优点就是可以在无显示设备的环境下稳定运行****！**

**环境配置**

典型的LAMP配置，网上资料很多，当然你也可以（[点此围观](http://www.besteric.com/?p=554)）

**搭建本地ShowSlow测试平台**

这个我之前在Windows Server 2003搭建过（[点此围观](http://www.besteric.com/?p=479)），但是这次在Ubuntu下还是有所区别的（**所有命令都在终端输入**）



	
  1. sudo mkdir /var/www/showslow (建立一个空文件夹)

	
  2. sudo svn checkout http://showslow.googlecode.com/svn/trunk/ /var/www/showslow (将ShowSlow的源代码下载到本地，这一步会报错连接不上http://svn.facebook.com，首先要感谢国家，其次要感谢功夫网，最后我要说的是无视...)

	
  3. sudo mv config.sample.php config.php (修改文件夹名)

	
  4. sudo gedit config.php

	
  5. 根据实际情况修改**$db,$user,$pass**可以根据实际情况修改

	
  6. 按照上一步修改的数据创建相应的Mysql数据库



> //以root用户权限进入mysql
** mysql -uroot -p**
//创建一个数据库，名字和第二步你填写的保持一致
** create database ‘DBName‘;**
//切换到新建的数据库
** use ‘DBName’;**
// 将ShowSlow文件夹的tables.sql（数据库表）导入到新建的数据库中，注意无分号
**source /var/www/showslow/tables.sql**
//查看下是否导入成功了，貌似有个表名叫ShowSlow2，汗
** show tables;**



**自动化脚本**

这个是我们这个框架最重要的部分，具体请参考[Sergey Chernyshev](http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/)的博客以及自动化脚本作者[Aaron Kulick](http://www.linkedin.com/in/xiphoidindustries) ，现在最新的Showslow的子文件夹automation有三个文件——**monitor.sh** (配置文件) **test_harness.pl** （自动化脚本） **ReadMe**（**框架说明文件，强烈推荐各位童鞋仔细阅读，因为这关系到后面脚本的修改问题**）

**Xvfb**

sudo apt-get install xvfb

Xvfb :1 -screen 0 1152x864x24 +extension RANDR


> 我在此处发现报错：**[dix] Could not init font path element /usr/share/fonts/X11/cyrillic, removing from list!**

通过Google大神的帮助，解决办法很简单，安装这个需要的字体——**sudo apt-get ****install**** xfonts-****cyrillic**


**创建一份测试页面列表**

sudo gedit /var/www/URLs （在Apache下新建一个URLs列表，注意每一个链接为单独的一行)

**创建一份Firefox测试专用的Profiles**

/usr/lib/firefox-3.6.3/firefox -profilemanager

首先我们要修改Firefox的application.ini文件最后一段，避免Firefox崩溃后发送报告


> 

> 
> [Crash Reporter]
> 
> 

> 
> Enabled=**0**


其次就是修改测试专用的Profiles的prefs.js，这个很关键，要设置一些Firefox属性才能让测试更好的进行下去，ShowSlow的论坛有推荐配置，（[猛击这里](http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7)）


> gedit /home/eric/FFProfiles/prefs.js

## PREFS.JS RECOMMENDATIONS (AUTOMATION)

**#do not let automated firefox manipulate the profile extensions (auto **

**update) **

user_pref("extensions.update.enabled", false);

user_pref("extensions.update.notifyUser", false);

**#disable session restore on crash (do not want stale/old tabs) **

user_pref("browser.sessionstore.resume_from_crash", false);

**#do not let javascript resize the window **

user_pref("dom.disable_window_move_resize", true);

**#do not let javascript manipulate context menus (automation) **

user_pref("pref.advanced.javascript.disable_button.advanced", false);

**#do not show me pop-up block messages (screenshot related) **

user_pref("privacy.popups.showBrowserMessage", false);

**#do not warn for weak SSL or mixed SSL/HTTP content: **

user_pref("security.warn_entering_weak", false);

user_pref("security.warn_viewing_mixed", false);

**#firebug prefs **

user_pref("extensions.firebug.allPagesActivation", "on");

user_pref("extensions.firebug.net.enableSites", true);

user_pref("extensions.firebug.defaultPanelName", "YSlow");

user_pref("extensions.firebug.previousPlacement", 1);

**#yslow prefs **

user_pref("extensions.yslow.autorun", true);

user_pref("extensions.yslow.beaconInfo", "grade");

user_pref("extensions.yslow.beaconUrl", "**http://localhost/showslow/beacon/yslow/"); **

user_pref("extensions.yslow.optinBeacon", true);


**配置本地YSlow**



	
  1. 打开Firefox输入：about:config（我保证会很小心的）

	
  2. filter中输入：yslow

	
  3. 修改以下三条数据



> extensions.yslow.beaconUrl = **http://localhost/showslow/beacon/yslow/**

如果测试和服务器不在同一机器上，请将**localhost**改成实际地址

**extensions.yslow.beaconInfo = grade**

**extensions.yslow.optinBeacon = true**


	
  4. 重启Firefox，have fun  :)


还等什么？开始你的测试之旅吧，查看测试报告的URL是：http://localhost/showslow/


> **在这个地方遇到了一个问题就是所有配置都是正确的情况下，ShowSlow依然接收不到YSlow发送的测试数据，后来在RedHat的服务器上搭建环境同样遇到了这个问题，经过SA白非童鞋的帮助查看Apache的报错日志（/var/log/apache2/error.log）发现罪魁祸首是脚本中需要的几个模块必须是PHP5.2以上的版本，遂升级PHP至最新版本可解决问题。**


**test_harness.pl**

这个脚本是用Perl语言写的，我也是刚接触这个语言，但是我还是推荐各位童鞋看看他的文件结构，老外写的代码阅读起来还是比较舒服，附带着大量注释方便我这样的小白也能轻松理解每个函数的意义。我们主要需要用到的是display，profile，source这三个属性（具体作用ReadMe有解释），可以参考下我运行这个脚本的方式：

perl test_harness.pl -display=:1 -source http://localhost/URLs -profile /home/eric/FFProfiles


> Can't locate POE.pm in @INC (@INC contains: /etc/perl /usr/local/lib/perl/5.10.1 /usr/local/share/perl/5.10.1 /usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.10 /usr/share/perl/5.10 /usr/local/lib/site_perl .) at test_harness.pl line 100.
BEGIN failed--compilation aborted at test_harness.pl line 100.

这个问题纠结了我好几天，百思不得其解，关键还是我第一次使用Linux和Perl，对于很多报错信息都可以熟视无睹（请鄙视我），最后请教了Sergey童鞋，才恍然大悟原来是没有安装POE（请再次鄙视我），解决办法参考如下：

**sudo perl -MCPAN -e shell （sudo很关键啊，内牛满面）**

**cpan> install POE**

**如果提示YAML没有安装 (类似于XML的数据描述语言)**

**cpan>install YAML**

**cpan> exit**


这个时候自动化脚本已经开始运行了，我们可以通过外部机器访问虚拟机搭建的ShowSlow查看测试结果 (Ubuntu使用ifconfig查看本机IP地址，注意虚拟机网卡要设置成Bridged方式)

**monitor.sh**

自动调用之前编写test_harness.pl脚本，当我们将调用test_harness的一些参数添加进monitor后使用Linux的Cron就可以实现自动化测试了：）


> 注意将脚本文件夹的绝对路径赋值给Xvfb_PIDFILE，因为每次执行的时候系统会自动生成一个__xvfb.pid


2010-7-1日更新

在Ubuntu和RedHat环境搭建下的Showslow平台点击测试URL进入Detail Page发现“Measurements over time”和“YSlow measurements history”两个区域的图形绘制不出，自己找了很久一直没有头绪，只好求助于ShowSlow的作者Sergey...
1 确保config.php文件中的$showslow_base=http://localhost/showslow/
2 查看phpinfo()确保Apache的模块mod_rewrite加载并启用了


> 1 Enable mod_rewrite

sudo a2enmod rewrite ;

2 change all of  "**AllowOverride none**" to "**AllowOverride All**"

sudo vim /etc/apache2/sites-enabled/000-default ;

3 Restart apache2

sudo /etc/init.d/apache2 restart;


刷新一下页面图形是不是回来了：）

在实际工作中我们还遇到了一个不大不小的问题就是有些页面需要登录才能访问，而我们的测试环境又是全自动化的，所以在这里需要用到Firefox的一个插件叫做Greasemonkey，可以执行JS脚本，这样只需要我们编写一个自动登录的小JS脚本，然后在Greasemonkey中设置那几个URL需要使用这个脚本，这样每次访问需要登录的界面就会自动登录了。

**参考文档（部分需要翻墙）：**

[http://en.wikipedia.org/wiki/Xvfb](http://en.wikipedia.org/wiki/Xvfb)

[http://developer.yahoo.com/yslow/](http://developer.yahoo.com/yslow/)

[http://www.showslow.com/](http://www.showslow.com/)

[http://www.ubuntu.com/](http://www.ubuntu.com/)

[http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/](http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/)

[http://www.linkedin.com/in/xiphoidindustries](http://www.linkedin.com/in/xiphoidindustries)

[http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7](http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7)

--EOF--
