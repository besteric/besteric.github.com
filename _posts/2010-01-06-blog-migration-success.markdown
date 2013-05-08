---
author: admin
comments: true
date: 2010-01-06 10:36:38
layout: post
slug: blog-migration-success
title: Blog Migration Success
wordpress_id: 300
categories:
- 内牛满面
---

昨天突发奇想想把05年至今的博客全部搬到目前的博客，可是之前的博客太零散了，MSN Spcae、Qzone（8要BS我）、网易博客都有一大部分残存，好在Google了一下发现可以利用强大的网易博客复制功能，先将MSN/QQ两个博客复制到网易，然后再通过网易中转下。

问题就出在网易这个中间环节了，由于我之前采用的是[yhustc](http://www.yhustc.com/)用python编写的一个搬家工具，但是年代久远已经失效，自己查看了下源代码发现解决不了遂放弃，继续Google了一下发现[zoomao](http://zoomao.info/2008/11/23/move-to-wordpress.html)可以通过一个Blog backup备份的工具先将网易的博客已RSS形式保存下来，然后再用WordPress的Rss导入功能。

Blog Backup是一个收费软件，最新版本的是2.0，但是我查看了更新目录发现老版本就可以满足我的需求遂继续Google了下所谓的“绿色版”，不过将网易的博客Rss保存后是不能直接导入的，需要注意以下三个地方：

每篇文章正文前面都有<![CDATA[ 字符，而结尾都有]]>字符

<description><![CDATA[<P>正文P>]]></description>，而标题则是<title><![CDATA[标题]]></title>

同时Rss里面充斥了大量Html空格也就是&nbsp;

非常简单的办法：用记事本编辑打开这个Rss.xml文件，然后分别查找<![CDATA[        ]]>     &nbsp  三个字符全部替换空白

大功告成

PS：搬家完毕发现一个典型问题，偶的大部分图片全挂了，算是明白了一个道理，鸡蛋别放在一个篮子里面
