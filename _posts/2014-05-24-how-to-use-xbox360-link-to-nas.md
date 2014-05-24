---
layout: post
title: "Xbox360安装FreeStyle访问Nas，实现远程无盘游戏"
description:
categories:
- 偶尔惊喜
tags:
- xbox360
- freestyle
- nas


---

自从折腾了一台黑群晖，就想把家里所有数码设备都折腾进Nas共享链路里面，于是又翻出了百年不开机的Xbox360。

**准备事宜**

1 Xbox360是老款日版双65nm豪华版（内置60G硬盘），买来的时候就已经刷好了自制系统，应该是最早那一批JTag破解机器（非脉冲破解），可以安装著名的第三方桌面FreeStyle，这个是玩转其他东东的前提，没有破解的同学就不用往下看了！

2 将Xbox360与Nas通过路由器（我使用的是[Cisco EA4500](http://www.besteric.com/2013/06/01/ea4500-openbox/)）连接在同一个局域网内，虽然Xbox内置网卡仅为百兆网卡，但是据不可靠消息说Xbox在游戏过程的数据传输速度不会超过5MB/S，所以百兆网卡玩游戏是不会有任何瓶颈的。

3 折腾的心，以及对于自制系统有一些基础的知识...

----

**具体操作**

1 打开群晖NAS系统的Windows共享协议（SMB）

群晖会告诉你局域网的机器如何访问它，比如我的Nas名字就叫MagicWorld

![image](http://gtms04.alicdn.com/tps/i4/T1EtUJFPFXXXb4zN_o-600-342.png)

2 安装FreeStyle

国内著名玩家@TJ逍遥有汉化最新版（也是最后一版）3.0.775，[直接点此下载](http://t.cn/z8QJru5)。用U盘拷贝至Xbox360内置硬盘（使用XexMenu应用），然后执行Default.xex文件进入FreeStyle。

![image](http://gtms01.alicdn.com/tps/i1/T1XyZlFQldXXaOhx_o-600-338.jpg)

3 设置Connectx

进入设置目录，选择「外挂程式」的「Connectx」，远端电脑名填写群晖系统的名称（我的是MagicWorld），共用资料夹名就是你希望Xbox360能够访问的共享文件夹（群晖需要设置好共享权限），用户名和密码填入群晖的账户。

![image](http://gtms02.alicdn.com/tps/i2/T1SWgJFH4aXXaOhx_o-600-338.jpg)

重启下FreeStyle，如果成功链接到NAS的话，你可以在文件管理找到「ConX」这个目录，点进去就是你映射的共享目录（我共享的是Transmission文件夹）

![image](http://gtms04.alicdn.com/tps/i4/T1AywEFFxcXXaOhx_o-600-338.jpg)

4 将NAS的游戏目录添加至扫描路径

这个时候再去「内容」重新「管理游戏路径」，将ConX以及Xbox自带硬盘内的文件路径添加进去

![image](http://gtms01.alicdn.com/tps/i1/T1JGwJFFlaXXaOhx_o-600-338.jpg)

等待它自动去卖场下载游戏封面，截图之类的，就可以在游戏中看到久违的游戏菜单了（这就是我为什么花时间折腾啊，自动刮削器有木有），而且他还会自动将XBLA和Xbox360游戏区分开来，多么智能啊！！！

![image](http://gtms02.alicdn.com/tps/i2/T1P8cDFJdcXXaOhx_o-600-338.jpg)

![image](http://gtms04.alicdn.com/tps/i4/T1Xy3pFNdcXXaOhx_o-600-338.jpg)

![image](http://gtms01.alicdn.com/tps/i1/T1jxkIFONXXXaOhx_o-600-338.jpg)

5 测试运行NAS的游戏

测试下我存放在NAS的XBLA游戏「Max The Curse of Brotherhood」，完美运行，就是在过场动画加载的时候稍微有点慢，不知道是不是NAS的缘故，不过游戏过程完全没有任何卡顿。

![image](http://gtms02.alicdn.com/tps/i2/T11a3JFHBaXXaOhx_o-600-338.jpg)

![image](http://gtms03.alicdn.com/tps/i3/T1usoHFNhbXXaOhx_o-600-338.jpg)

基本上Xbox360设置到这里就告一段落了，终于可以摆脱内置60G硬盘的限制了，轻松管理存放在NAS的游戏库了：）

注意事项：

* FreeStyle自带游戏截图功能，并会将其保存至/FreeStyle/Plugins/UserData/XXXXXXXX(游戏编号)/Screenshots/ 目录下 

* 天气插件需要自行去worldweatheronline.com申请一个免费API 

如果愿意折腾的同学还可以考虑：

* 使用Link实现类似浩方对战平台的局域网模拟功能，和其他好基友对战（国内貌似有些联机群）

* 将Xbox360当做媒体播放器使用，我播放器泛滥成灾就不折腾了：）


--EOF--