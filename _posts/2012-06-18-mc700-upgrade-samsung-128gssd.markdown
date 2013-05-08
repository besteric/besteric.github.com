---
author: admin
comments: true
date: 2012-06-18 13:05:16
layout: post
slug: mc700%e5%8d%87%e7%ba%a7%e4%ba%86%e4%b8%89%e6%98%9f830-128gssd
title: MC700升级了三星830 128GSSD
wordpress_id: 927
categories:
- 蛋疼的事
---

MC700升级到8G内存后，发现速度还是不太理想，看来目前电脑的最大瓶颈依然是I/O。虽然对SSD一直蠢蠢欲动，但是无奈价格实在有点肉痛，不过好消息是最近128G的SSD价格持续走低，特别是两大爆款——[镁光 M4](http://s8.taobao.com/search?q=%C3%BE%B9%E2+M4&cat=0&pid=mm_16933576_0_0&mode=23&commend=1%2C2) /[三星 830](http://s8.taobao.com/search?q=%C8%FD%D0%C7+830&cat=0&pid=mm_16933576_0_0&mode=23&commend=1%2C2)...之前一直都打算入M4，不过看了830的介绍只有6mm的厚度，马上倒戈棒子阵营了，关键他还比M4便宜几十，不愧为性价比之王。

每一次消费背后都有一个冲动的我，特别是今年[WWDC](http://itunes.apple.com/cn/podcast/apple-keynotes-1080p/id509310064?mt=2)公布了Retina屏幕的MBP后，我发现手上的MC700即将面临淘汰的尴尬局面...一咬牙入了[三星830 128G SSD](http://s8.taobao.com/search?q=%C8%FD%D0%C7+830&cat=0&pid=mm_16933576_0_0&mode=23&commend=1%2C2)。考虑到128G的SSD对于我这个伪高清党实在有点蛋疼，于是考虑将原装的HDD机械硬盘放在光驱位，又经过大量搜索，确定了MBP的光驱托架最佳解决方案——[Nimitz](http://s8.taobao.com/search?q=Nimitz+sata+3&cat=0&pid=mm_16933576_0_0&mode=23&commend=1%2C2)

安装过程本来还想来个图文解说，不过我太懒了，而且网上已经有[非常详细的图文解说了](http://bbs.weiphone.com/read-htm-tid-4265042.html)，我就不画蛇添足了...值得一提的是拆光驱确实需要胆大细心，其他的就是纯体力劳动了..

由于手头上没有U盘启动盘，所以直接将SSD放在硬盘位，HDD放在光驱位，然后按option开机，选择Recover盘直接从Time Machine恢复系统至SSD（别忘了先格式化下）...差不多耗时2个小时，系统无缝迁移至SSD上，这个不得不赞叹下水果的人性化操作。最后下载[Trim Enabler](http://www.groths.org/?page_id=322)打开Mac的[Trim](http://ask.zol.com.cn/q/11434.html)功能，升级告一段落。

使用[AJA System Test](http://www.aja.com/ajashare/AJA_System_Test_v601.zip)测试硬盘速度，SSD数据实打实的比HDD快4倍...实际使用的过程来说，应用程序基本是秒开级别，虚拟机开启大致再3秒时间....初步让我觉得这钱花的还算值..

![](http://ww4.sinaimg.cn/large/661e5653gw1du15a9vwgsj.jpg)
