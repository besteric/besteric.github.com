---
author: admin
comments: true
date: 2012-05-03 17:26:42
layout: post
slug: '%e5%8d%9a%e5%ae%a2%e8%bf%81%e7%a7%bb%e8%87%b3amazon-ec2%e4%b8%bb%e6%9c%ba'
title: 博客迁移至Amazon EC2主机
wordpress_id: 904
categories:
- 蛋疼的事
tags:
- amazon ec2
- wordpress
---

一直以来本博客都是挂靠在朋友的国内主机，结果去年底被告知需要备案，还好[@heysql](http://heysql.com/)再次收留了我的博客，不过[@heysql](http://heysql.com/)的主机及其不靠谱，基本每天都要重启主机，所以又萌生了迁移主机的想法，最近终于腾出时间迁移博客了...顺便总结下自己的一些经验

**Introduction**



	
  1. 建议Region选择为Tokyo或者Singapore机房，有利于国内用户访问

	
  2. Launch instances建议直接使用Community AMIs，大大节省环境部署安装时间

	
  3. Elastic IP不绑定EC2主机，闲置，直接使用，都是要收费的...换言之，绑定不使用不收费

	
  4. 预装bitnami的AMI镜像主机，SSH的用户名是bitnami，而不是常见的ec2-user/ubuntu


**Register EC2**

参照网上泛滥成灾的各种[注册教程](http://www.bityun.com/archives/42)，还是比较简单的注册aws.amazon.com帐号。

**Choose Region**

首先在左上角的Region选择一个速度较快的机房，我测试了US East California/Singapore/Tokyo的速度，相对而言感觉东京的机房速度还是快点，所以就选它了...值得一提的是EC2所有区域的主机不是互通的，Instance也是相对独立的。

![](http://img01.taobaocdn.com/tps/i1/T1rb4eXa4yXXaRQDsX-1167-668.png)

**Launch instances**

网上搜到的大部分教程都是从“Quick Start”开始的，其实这个一点都不Quick，建议大家直接选择“Community AMIs”，这是网友上传已经部署环境的镜像文件，比如我需要的博客程序，直接搜索“wordpress”，Viewing选择“EBS Images”，然后挑一个wordpress版本最新的即可，比如我选择的就是ami-f68c3df7这个镜像文件...然后在Instance Details里面一定要记得选择**instance Type: Micro (t1.micro,613MB)**，这个是免费一年的主机，其他都是要收费的，其他选项都默认，需要注意的是Configure Firewall一定要自己建立一个规则，开启SSH端口22，http端口80，SSL端口443，PPTP VPN端口1723....最后确认下果断Launch

![](http://img04.taobaocdn.com/tps/i4/T1pH4IXktAXXaRQDsX-1167-668.png)

**Elastic IPs**

每一个Instance都会获得一个Public DNS，譬如本博客ec2-XX-XX-XX-XX.ap-northeast-1.compute.amazonaws.com，但是很多网友测试Instance重启后这个Publick DNS会随机改变，这就需要我们使用Elastic IPs绑定一个静态IP，这样Instance重启后也不会改变Publick DNS。**值得一提的是Elastic IPs只能绑定再EC2的主机上，如果不绑定或移作它用都会被Amazon惩罚性的收费**。

**Visit Web Page**

由于我们选择的镜像文件是预装了[bitnami](http://bitnami.org/)，它非常贴心的默认安装了Wordpress，Apache，Mysql，Phpmyadmin等常用环境...直接使用Public DNS就能访问到默认的主页，加一个/wordpress/后缀就能看到预装的wordpress...是不是很贴心啊：）剩下的就是迁移数据了...不过在此之前我们还是绑定下域名了，看着那一长串的Public DNS太闹心了...

**CNAME**

besteric.com注册于[Godaddy](http://www.godaddy.com/)，所以我们还是去他的后台操作下吧，这里要注意的是我们建议直接使用CNAME绑定Publick DNS，而不是将A绑定到Elastic IP，[因为Elastic IP是按需收费的](http://blog.sina.com.cn/s/blog_54a63ef80100o2yw.html)，更关键的是同在EC2云里的主机使用Public DNS访问你的主机时，会直接得到Amazon分配给你的内网地址进行通讯，而不需要经过公网再绕回EC2的云里，[网上有同学已经详细分析过这个问题了](http://www.stevenringo.com/elastic-ip-on-amazon-ec2-why-using-a-cname-is-better-than-an-a-record/)。但是如果不绑定A的话besteric.com就无法访问了，这个暂时想到的解决办法是绑定到第三方免费域名，再解析一次跳转回来....

**[SSH](http://wiki.bitnami.org/BitNami_Cloud_Hosting/Servers/SSH)**

后续的很多操作都需要SSH到EC2主机上进行命令行操作，简单介绍下Mac如何SSH，再终端使用命令“ssh -i /Users/besteric/.ssh/blogkey.pem **bitnami**@XX-XX-XX-XX-XX.ap-northeast-1.compute.amazonaws.com”，其中密钥文件pem路径请自行选择，**值得注意的是bitnami镜像的登录名是bitnami，而不是ec2上面写的ec2-user，抑或是网上说的ubuntu**...这个地方我折腾了最久，还好是[看了文档才找到解决办法](http://wiki.bitnami.org/BitNami_Cloud_Hosting/Servers/SSH?highlight=ssh#How_to_connect_to_my_server)。不过bitnami这个用户的权限并不是root权限，如何使用root权限请[参照此文](http://www.flash888.com/?p=233)。

[Phpmyadmin](http://zh.wikipedia.org/wiki/PhpMyAdmin)

我也是第一次使用这个Mysql的数据库管理工具，不过我们还是先看看[bitnami对于Phpmyadmin的说明](http://wiki.bitnami.org/Components/phpMyAdmin_and_phpPgAdmin)，默认情况下只能再localhost访问Phpmyadmin，所以需要按照这个[指示](http://wiki.bitnami.org/Components/phpMyAdmin_and_phpPgAdmin#How_to_enable_phpMyAdmin_or_phpPgAdmin_to_be_accessed_remotely.3f)修改，在此不再赘述...修改完后访问路径是you-domain/phpmyadmin/，第一次登录需要网页认证，用户名和密码是administrator/bitnami，进去以后需要输入Mysql的用户名和密码，默认是root/bitnami...默认的wordpress数据库名是bitnami_wordpress，选择以后直接将你的sql后缀文件导入即可...我在这里没有遇到什么问题就不多说了。

**Wordpress**

首先使用FTP将备份的wordpress文件上传至/opt/bitnami/apps/wordpress/htdocs/ 文件夹下，Mac用户推荐使用Transmit，选择SFTP方式链接，密码直接选择PEM密钥文件，轻松搞定...然后尝试访问you-domain/wordpress/是否正常，如果遇到错误“Error establishing a database connection！”，那肯定就是数据库错误了，有几种可能：1 数据库损坏，需要修复，[尝试下这个方法](http://plus-now.com/?p=392) 2 数据库用户名/密码错误，需要修改/opt/bitnami/apps/wordpress/htdocs/wp-config.php 文件，至此解决上述问题终于可以看到我们可爱可亲的博客界面了...

**Virtual Host**

由于本人及其懒，所以想besteric.com直接链接到blog，而不是besteric.com/wordpress/这种笨挫逊的方式，这就需要用到Apache虚拟映射了，具体的请参考[这里](http://wiki.bitnami.org/Components/Apache?highlight=apache#How_to_create_a_Virtual_Host.3f)，修改/opt/bitnami/apache2/conf/httpd.conf 的时候最好顺手将**AllowOverride None全局替换为AllowOverride All**，为了后续操作.htacces文件提供方便....一切就绪后就可以直接访问besteric.com映射到blog了。

至此我们顺利的将博客迁移至Amazon EC2主机上，如果你还想利用它打探下处于水生火热的帝国主义国家的秘密的话，[欢迎使用PPTP VPN教程](http://www.myvoipapp.com/blogs/yxh/2011/02/24/amazon-ec2ubuntu%E7%B3%BB%E7%BB%9F%E4%B8%8B%E6%90%AD%E5%BB%BAvpn%E7%8E%AF%E5%A2%83/)

参考资料(太多了，简单列下我收藏的):



	
  1. [Bitnami Wiki](http://wiki.bitnami.org/)

	
  2. [AMAZON EC2学习笔记(一)——EC2 INSTANCE的搭建](http://www.giroro.com/amazon-ec2-study-notes-1/)

	
  3. [开博历程](http://pananq.com/index.php/tag/ec2/)

	
  4. [博客迁移至Amazon EC2](http://www.lovelucy.info/migrate-blog-to-amazon-ec2.html)

	
  5. [Amazon AWS 漫游指南](http://yinhm.appspot.com/2010/10/amazon-ec2-micro-instance-and-tunnel-guide)

	
  6. [用Amazon EC2搭建免费WordPress博客及SSH](http://neolee.com/web/use-amazon-ec2-for-wordpress-and-ssh/)

	
  7. [將ec2-user轉成root身份運行方法](http://www.flash888.com/?p=233)

	
  8. [Amazon EC2 Ubuntu折腾笔记](http://lanhl.com/2011-amazon-ec2-ubuntu-lamp-wordpress.html)

	
  9. [Amazon EC2/Ubuntu系统下搭建VPN环境](http://www.myvoipapp.com/blogs/yxh/2011/02/24/amazon-ec2ubuntu%E7%B3%BB%E7%BB%9F%E4%B8%8B%E6%90%AD%E5%BB%BAvpn%E7%8E%AF%E5%A2%83/)

	
