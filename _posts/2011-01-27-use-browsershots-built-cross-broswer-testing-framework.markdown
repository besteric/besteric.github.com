---
author: admin
comments: true
date: 2011-01-27 19:27:45
layout: post
slug: '%e6%9c%ac%e5%9c%b0%e6%90%ad%e5%bb%ba%e5%a4%9a%e6%b5%8f%e8%a7%88%e5%99%a8%e5%85%bc%e5%ae%b9%e6%80%a7%e6%b5%8b%e8%af%95%e5%b9%b3%e5%8f%b0browsershots'
title: 本地搭建多浏览器兼容性测试平台Browsershots
wordpress_id: 731
categories:
- Performance
tags:
- browsershots
- Test
---

** **

** **

**工具介绍**

[Netpbm](http://netpbm.sourceforge.net/)：非常给力的命令方式图像处理程序，支持WINDOWS、LINUX及部分UNIX系统，主要用于批量转换图片格式

[Django](http://www.djangoproject.com/)：python语言编写的开源web开发框架，本平台只能使用1.0.4版本（目前最新的是1.3 beta1）

[PostgreSQL](http://www.postgresql.org/)：一种特性非常齐全的自由软件的对象-关系型数据库管理系统，当然也是开源的

[Browsershots](http://browsershots.org/)：通过在不同操作系统下用不同浏览器渲染您的网页，然后获取截图的方法来测试网站的浏览器兼容性，最神奇的是作者(@jcrocholl)现在还是学生

**环境介绍**

Vmware 7.0.1 + Ubuntu 10.0.4 + Python 2.6 + Django 1.0.4

**截图服务器搭建**

安装Netpbm


> sudo apt-get install netpbm


安装Django

官网上最新版本是1.3 Beta1，但是Browsershots V0.4开发框架使用的还是1.0.4版本


> svn checkout http://code.djangoproject.com/svn/django/tags/releases/1.0.4 django

cd django

sudo python setup.py install


安装ShotServer

ShotServer相当于截图服务器,用于接收和发送截图命令,目前是0.4版本，执行安装操作后会自动拷贝文件夹至/usr/local/lib/python2.6/dist-packages/shotserver04


> svn checkout http://browsershots.googlecode.com/svn/trunk/shotserver

cd shotserver

sudo python setup.py install


安装PostgreSQL

Ubuntu 9.0版本后psycopg作者停止更新老版本了,所以只能用最新版本,这也需要在后面的操作进行相关修改!


> sudo apt-get install postgresql python-psycopg2


创建数据库用户

eric可以为任意用户名，其实可以设定角色为超级用户（superuser）


> sudo -u postgres createuser eric -W –P

Enter password for new role:123456

Enter it again:123456

Shall the new role be a superuser? (y/n) y


备注：修改已有用户密码可以使用如下命令


> sudo –u postgres psql

postgres =# ALTER USER eric WITH PASSWORD ‘123456’;

postgres=# \q


创建数据库


> createdb shotserver04


创建数据库表

这个步骤非常关键，我们在初次安装的过程中卡在这里很久，因为之前提到的psycopg非常古老已经停止更新了，取而代之的是postgresql_psycopg2，所以我们首先需要修改安装目录下的/usr/local/lib/python2.6/dist-packages/shotserver04里面的setting.py里面的配置：


> DEBUG = True

DATABASE_ENGINE = 'postgresql_psycopg2'           # 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.

DATABASE_NAME = 'shotserver04'             # Or path to database file if using sqlite3.

DATABASE_USER = 'eric'             # Not used with sqlite3.

DATABASE_PASSWORD = '123456'         # Not used with sqlite3.

DATABASE_HOST = 'localhost'             # Set to empty string for localhost. Not used with sqlite3.

DATABASE_PORT = ''             # Set to empty string for default. Not used with sqlite3.


如果喜欢中文界面的同学请修改setting.py下列属性


> TIME_ZONE = 'Asia/Shanghai'

LANGUAGE_CODE = 'zh-cn'

同时在LANGUAGE里面加入一行(‘zh-cn’, u’简体中文’)


除此之外还需要按照如下信息将shotserver文件夹所有psycopg字段换成psycopg2，最后将DEBUG属性改成True，因为作者的逻辑非常给力(不解释)



	
  * In the shotserver folder, type in: grep -ri psycopg . (后面有一个"点")

	
  * Go through those files and change them all to "psycopg2"

	
  * The default django that comes with ubuntu at the moment(ubuntu10.10)      uses psycopg in some places and psycopg2 in others. May have to change      references to "psycopg" in file /usr/local/lib/python2.6/dist-packages/django/db/backends/postgresql/base.py to "psycopg2"



	
  * A helpful thing to do is change DEBUG to "True" in settings.py so you can see the error messages.


做完上述操作终于可以正常创建数据库表了（我们大部分时间都在研究上述的故障了）


> cd shotserver/shotserver04
sudo python ./manage.py syncdb


然后按照提示设定admin账户

创建保存截图文件夹

如果你没有修改setting.py的路径设置，可以直接用下面的命令创建文件夹


> sudo mkdir -p /var/www/v04.browsershots.org/png


当然保存png图片的文件夹必须有写的权限


> sudo chown eric:123456 /var/www/v04.browsershots.org/png


**使用开发服务器快速开始**** **

Django本身就提供一个开发服务器，所以我们一开始没必要使用Apache或者其他服务器，可以先在本地调试成功后在转换到Apache等服务器上运行

运行开发服务器


> cd /usr/local/lib/python2.6/dist-packages/shotserver04

sudo python ./manage.py runserver


以上的命令会在本地提供服务，地址是127.0.0.1:8000，如果你想让外部用户也能访问到你机器上的内容，你可以加上一个本机对外IP地址


> sudo python ./manage.py runserver 10.13.17.46:8000


OK,大功告成，终于可以看到简洁的截图服务器运行界面了，不过因为还没有截图客户端所以会提示No active screenshot factories.那我们接下来的工作就是搭建客户端了

[![](http://www.besteric.com/wp-content/uploads/2011/01/1.jpg)](http://www.besteric.com/wp-content/uploads/2011/01/1.jpg)

**截图工厂搭建**** **

BrowserShots其实一个分布式系统，客户端（ShotFactory）可以为任意系统（Linux/Win/Mac）的任何浏览器（IE/Firefox/Chrome/Safari），由于我只使用过Linux版本的ShotFactoryLinux，简单介绍下吧J

前期准备


> sudo apt-get install tightvncserver xfonts-base netpbm xautomation scrot subversion


安装ShotFactory


> svn checkout [http://browsershots.googlecode.com/svn/trunk/shotfactory](http://browsershots.googlecode.com/svn/trunk/shotfactory)


配置环境

首先运行下tightvncserver，它会自动生成一个文件类似 ~/.vnc/xstartup，我们需要修改以下三行代码来保证截图工厂能够稳定的运行在白色背景下并且只有一个窗口。


> #!/bin/sh

Xsetroot –solid “#FFFFFF”

x-window-manager &


如果没有作用的话，我们需要修改这个文件的读写权限


> Chmod a+x ~/.vnc/xstartup


注册截图工厂

浏览器访问[http://127.0.0.1:8000/admin/factories/factory/add](http://127.0.0.1:8000/admin/factories/factory/add)


> Factory Name: besteric

Administrator: eric

Operating system: Ubuntu 10.0.4

Width: 1280 Height: 800 Bits per pixel: 16


保存后就能看到如下

[![](http://www.besteric.com/wp-content/uploads/2011/01/2.jpg)](http://www.besteric.com/wp-content/uploads/2011/01/2.jpg)

关联截图工厂的浏览器

浏览器访问[http://127.0.0.1:8000/browsers/add/](http://127.0.0.1:8000/browsers/add/) 会自动识别浏览器类型、版本号、Flash版本等信息，只需填写以下两行信息


> Factory: besteric

Password: 123456


运行截图工厂


> cd /home/eric/shotfactory/

screen -L python shotfactory.py -s http://127.0.0.1:8000 -f besteric -P 123456


浏览器访问[http://127.0.0.1:8000/](http://127.0.0.1:8000/) 可以看到已经可以识别出我们刚注册的浏览器了

[](http://www.besteric.com/wp-content/uploads/2011/01/3.jpg)[![](http://www.besteric.com/wp-content/uploads/2011/01/4.jpg)](http://www.besteric.com/wp-content/uploads/2011/01/4.jpg)

测试页面

那我们就拿淘宝首页作为开山之测吧，输入[http://www.taobao.com/](http://www.taobao.com/) 提交后可以看到页面等待时间和过期时间等信息：

[![](http://www.besteric.com/wp-content/uploads/2011/01/5.jpg)](http://www.besteric.com/wp-content/uploads/2011/01/5.jpg)

当然我们也可以在终端看到更多的底层交互信息，过一会我们就能点击左上角的”Screenshots ”看到指定分辨率的截图了

[![](http://www.besteric.com/wp-content/uploads/2011/01/6.png)](http://www.besteric.com/wp-content/uploads/2011/01/6.png)

**其他**** **

截图工厂稳定运行在Windows和Mac系统下，这样就可以实现跨平台多浏览器的截图测试了，值得一提的是一个截图工厂如果有多个浏览器都可以同时注册J

如果要建立一个外网可以访问的Browsershots，就需要使用到Apache的mod_python模块来运行Django，有兴趣的同学可以参考最后的参考文档自行设置

**特别感谢**** **

UDC前端架构组的小马，乔花，季札以及国外的Niknah同学

**参考文档（部分需要翻墙）**** **

[http://code.google.com/p/browsershots/wiki/InstallServer](http://code.google.com/p/browsershots/wiki/InstallServer)

[http://code.google.com/p/browsershots/wiki/InstallFactoryLinux](http://code.google.com/p/browsershots/wiki/InstallFactoryLinux)

[http://www.tbdata.org/archives/721](http://www.tbdata.org/archives/721)

[http://groups.google.com/group/django-users/browse_thread/thread/915a73e17b2683f1](http://groups.google.com/group/django-users/browse_thread/thread/915a73e17b2683f1)

[http://blog.sina.com.cn/s/blog_3fa78b820100a217.html](http://blog.sina.com.cn/s/blog_3fa78b820100a217.html)

[http://jianlee.ylinux.org/Computer/%E6%9C%8D%E5%8A%A1%E5%99%A8/postgresql%E6%95%B0%E6%8D%AE%E5%BA%93%E5%85%A5%E9%97%A8.html](http://jianlee.ylinux.org/Computer/%E6%9C%8D%E5%8A%A1%E5%99%A8/postgresql%E6%95%B0%E6%8D%AE%E5%BA%93%E5%85%A5%E9%97%A8.html)









** **
