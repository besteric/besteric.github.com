---
layout: post
title: "make my custom nas"
description: 前段时间由于扫黄打非导致国内大部分PT站点上不去，闲着蛋疼的开始琢磨是否需要折腾一个NAS作为家庭存储+影音播放+脱机下载使用，于是自己组装了一台ITX机器刷了synology系统...
categories:
- 蛋疼的事
- 折腾数码
tags:
- synology
- 群晖
- NAS
- ITX


---

去年搬进新家的时候曾经折腾了一个[华为HG255D路由器](http://redirect.simba.taobao.com/rd?w=unionnojs&f=http%3A%2F%2Fre.taobao.com%2Feauction%3Fe%3DInbV5XuTBiHebLdhAWchHBWPN1SFdommWPt1fh5JBxyLltG5xFicOSZqewpHPyZzVuAX9KjHXqlrRF2mRoYw2w6%252F27l4VpYj72xyKpEWvWWB3ujUJI0OeA%253D%253D%26ptype%3D100010&k=e2e107a2b72ca1b1&c=un&b=alimm_0&p=mm_16933576_5054595_15502779)，刷了OPENWRT第三方固件，使用Transmission实现了PT脱机下载，并且功耗非常小可以7X24长时间开机，缺点也是非常明显的，由于采用USB 2.0外挂移动硬盘，I/O仅仅能维持2-3MB/S的速率...

最近看到很多网友晒单低成本组装ITX主机，刷群晖系统从而实现家庭NAS功能，身为不折腾不舒服斯基星球的人，我决定按照[link](http://show.smzdm.com/detail/60369)这个思路，自己也组装一台机器。

**配置清单:**

主板：ZOTAC/索泰 Mini-itx（板载 ATOM N230 CPU）
内存：2G DDR2 667/800
机箱：乔思伯 V6
电源：闲置19V/3.43A笔记本电源
优盘：闲置不知名品牌8G迷你优盘
硬盘：闲置巴法络 3.5寸 2TB 移动硬盘 拆机硬盘

---

下单购买主板+内存+机箱后，本来是打算将之前挂在在路由器上的500G 西部数据硬盘给拆了，结果Google下发现这款硬盘是将电路直接焊死在硬盘上的，拆开就基本报废了...只好退而其次的将家里闲置的巴法络2TB 3.5寸移动硬盘给拆了..

![image](http://gtms02.alicdn.com/tps/i2/T1H6klFJNaXXaUGAjo-600-626.jpg)

当初拆机看了下[这篇文章](http://show.smzdm.com/detail/10237)，发现很多细节不太清楚，拆了满头的汗就是拆不下来，最后无奈去Youtube搜到了一个[日本同学的拆机视频](https://www.youtube.com/watch?v=4-L0loK45Ck)，终于完美的将它拆下来了。

这是收到主板的图片,这块主板的优点非常明显：

* 板载ATOM N230 CPU
* 板载千兆网卡
* USB 2.0 *10 + HDMI + VGA + Sata3 *3 + eSata

价格居然还是异常惊人的160元（不含邮费12元），很多网友都是买来作为客厅HTPC或者NAS使用，反馈都说物超所值

![image](http://gtms03.alicdn.com/tps/i3/T1Gr7nFTJXXXaIyj2o-600-600.jpg)

这款主板只能使用6x6尺寸的CPU风扇，但是我购买的这个超速3风扇声音实在有够「静音」，最后还是没有使用上...

内存请注意千万别贪图便宜而购买AMD专用DDR2 667/800内存条，我就是一个活生生的教训，当天收到后测试了一晚上死活进不去PE系统，最后排查就是内存条的问题...

机箱采用的是目前比较流行的乔思伯V6 ITX专用机箱，外观非常漂亮，远看就是一个音箱低音炮大小，基本上没有任何动手能力的同学也能非常轻易的装机

![image](http://gtms01.alicdn.com/tps/i1/T1wxgeFQ0dXXaIyj2o-600-600.jpg)

最后装机完毕，调试安装群晖的DSM 5.0 4458系统没问题后，将其藏在了客厅电视柜的底部

![image](http://gtms02.alicdn.com/tps/i2/T1TJceFGheXXaIyj2o-600-600.jpg)

---

基本的硬件组装可以参考[Diors也有春天：420元DIY家用NAS](http://show.smzdm.com/detail/60369)

群晖系统的安装可以参考[NAS群晖DSM 5.0-4458 傻瓜安装教程](http://jy.smzdm.com/detail/20115)

有几点需要注意：

* 装机按照硬件配置直接买即可，内存、优盘、笔记本电源如果有闲置的最好
* 群晖系统引导文件Gnoboot最新的内核是10.5，如果升级至 DSM 5.0 4458 update 2（主要解决OpenSSL这个重大安全Bug）需要使用它引导进入系统


剩下的就是和正版群晖一样学会使用NAS，尽情去折腾吧...

* 安装了第三方Transmission + Flexget实现PT站点RSS订阅下载（再也不用手动下载种子再上传给TM了）
* 使用群晖官方套件DS Video为iPad/iPhone提供视频服务器
* 群晖模拟Time Machine实现Mac无线备份
* 提供各种格式共享高清片源给电视盒子播放