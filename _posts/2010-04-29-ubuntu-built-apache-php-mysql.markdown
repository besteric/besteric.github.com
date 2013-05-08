---
author: admin
comments: true
date: 2010-04-29 10:09:29
layout: post
slug: ubuntu%e6%90%ad%e5%bb%baapachephpmysql
title: Ubuntu搭建Apache+PHP+Mysql
wordpress_id: 554
categories:
- Performance
tags:
- Apache
- Mysql
- PHP
- SVN
- Ubuntu
---

**Ubuntu （10.04-beta2-desktop-i386）**

推荐各位童鞋先用VMware虚拟机安装Ubuntu，以免破坏本机的环境，具体安装就不说了，注意别将登陆密码忘了

**VMware Tools （8.1.4-227600）**

这个强烈推荐安装，鼠标可以无缝切换于本机和虚拟机，文件拖拽功能，剪贴板通用等等..

sudo apt-get install build-essential

sudo apt-get install linux-headers-2.6.32-19-generic （headers信息有差异，请用 uname -a 查看linux-headers版本）

tar -xzvf  VMwareTools-8.1.4-227600.tar.gz

cd vmware-tools-distrib

sudo ./vmware-install.pl

Enjoy——the VMware team ^^

**Apache**

sudo apt-get install apache2

然后按照提示即完成apahce的安装了。这里可以打开http://127.0.0.1 即可看到It works

**PHP5**

sudo apt-get install php5

sudo apt-get install libapache2-mod-php5

sudo /etc/init.d/apache2 restart 重启下Apache服务

**Mysql**
sudo apt-get install mysql-server 安装完成按提示设置root密码

**配置Apache、PHP支持Mysql**

sudo apt-get install libapache2-mod-auth-mysql

sudo apt-get install php5-mysql

sudo /etc/init.d/apache2 restart

**SVN**

sudo apt-get install subversion

-- EOF --
