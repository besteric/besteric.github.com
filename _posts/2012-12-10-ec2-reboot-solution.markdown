---
author: admin
comments: true
date: 2012-12-10 09:35:17
layout: post
slug: ec2%e4%b8%bb%e6%9c%bareboot%e9%87%8d%e5%90%af%e5%90%8e%e6%95%b0%e6%8d%ae%e5%ba%93%e8%a1%a8wp_options%e4%b8%adsiteurlhome%e5%9b%9e%e6%bb%9a%e8%a7%a3%e5%86%b3%e6%96%b9%e6%a1%88
title: EC2主机Reboot重启后数据库表wp_options中siteurl&home回滚解决方案
wordpress_id: 974
categories:
- 蛋疼的事
---

每次为了对搜索引擎友好，不得不把标题写的如此恶心，我有种淡淡的忧伤...

很久（大于三个月）没来博客除草了，今天百忙之中抽空访问了下博客，结果又遇到了熟悉的「Error establishing a database connection」，WTF，直接Reboot了整个EC2主机，重启后访问besteric.com发现CSS等资源文件全部丢失，看了下报错信息，原来hostip又rollback至Amazon EC2's Public DNS，只能手动去mysql修改bitnami_wordpress->wp_options->siteurl&home 解决。

思前想后这尼玛太屌丝了，肯定有个万恶的脚本每次重启后自动修改这两个字段，Google搜索「ec2 reboot bitnami wordpress rollback」字段，终于真相大白，[诚如这位老兄遇到的问题](http://answers.bitnami.org/questions/808/magento-base_url-reset-every-time-i-reboot-the-ec2-instance)，原来每次重启后updateip这个脚本都会自动重写hostip，解决办法也So easy，重命名这个脚本即可...值得注意的是我的bitnami居然有两个地方都有这段脚本，路径分别为 opt/bitnami/updateip + opt/bitnami/apps/wordpress/updateip ，修改前者不管用，必须修改wordpress里面的updateip才行...

至此你可以肆无忌惮的重启EC2主机了...

PS 1: @helloleo 推荐在wp-config.php里面直接写死，可以override掉数据库的值，有兴趣的同学可以尝试下

PS 2:下次遇到「Error establishing a database connection」这个问题，记得先查看下mysql日志，看看具体是什么原因导致的。

2012-12-19 Update

上个礼拜又遇到「Error establishing a database connection」，尝试使用phpmyadmin登录发现Mysql完全挂掉，SSH登录到服务器，手工修改WP-Config.php这个文件的将define('DB_HOST', 'localhost');「localhost」修改为「127.0.0.1」即可解决问题，之前有看到为什么如此修改，但是今天已经搜不到了...不知道哪位同学知道根本原因，麻烦回复下，谢谢了...[这篇文章给了几种方式](http://www.wpbeginner.com/wp-tutorials/how-to-fix-the-error-establishing-a-database-connection-in-wordpress/)，感觉还是很详细的，推荐下
