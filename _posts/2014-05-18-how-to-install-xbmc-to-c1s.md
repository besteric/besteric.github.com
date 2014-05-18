---
layout: post
title: "XBMC 13.1 Gotham beta1 with Letv C1S"
description: NAS折腾好了以后，局域网观看高清视频简直就是手到擒来，受限于电视盒子ARM平台，还是有必要强烈推荐下XBMC
categories:
- 偶尔惊喜
tags:
- NAS
- XBMC
- OTT
- 家庭网络


---

昨天完全弄好NAS后，我就尝试重新搭建家庭高清视频分享环境，主要还是基于客厅的乐视C1S电视盒子，我使用的是XBMC最新发布的xbmc-13.1-Gotham_beta1-armeabi-v7a.apk，可以直接去[官网下载](http://xbmc.org/download/)，强烈推荐13.0以后的版本，据说对于ARM平台有非常多的优化，甩12.X版本几条街不知道...

这是安装完后的界面，图标也更换为更洋气的黑色了

![image](http://gtms02.alicdn.com/tps/i2/T1fUCPFtXmXXXqjx_o-600-338.png)

然后安装豆瓣的刮削器（自动读取电影简介，演员，IMDB评分等信息）以及射手字幕插件神器，基本就完成了家庭影院的搭建，这是自动加载的电影信息（PT下载的电影基本都是符合标准命名规范的）

![image](http://gtms04.alicdn.com/tps/i4/T1bMBZFMFdXXXqjx_o-600-338.png)

写到这里突然忘记推荐大家使用[NFS](http://en.wikipedia.org/wiki/Network_File_System)格式作为NAS共享给XBMC使用（NFS在UNIX系统传输效率比SMB协议之类更为高效些），群晖设置相当简单，截图仅以DSM 5.0为例

控制面板-文件服务-WIN/MAC/NFS-开启NFS服务

![image](http://gtms01.alicdn.com/tps/i1/T1mWGJFIVhXXaEQh_o-600-343.png)

控制面板-共享文件—对于电影文件（Movies）开通NFS权限，客户端写 * 表示允许所有

![image](http://gtms03.alicdn.com/tps/i3/T1u7onFT4aXXc8Lizo-600-412.png)

到这边你以为就好了么? Too young,too simple...在OSX/iOS/Android下，对NFS协议的使用有个蛋疼的规定，非Root用户是不能使用Reserved Ports的，而NFS Server默认是不支持非Reserved Ports，具体表现就是XBMC可以找到你共享的文件夹，却没有权限访问它的子文件夹！

还好我们可以手工编辑配置文件绕开这个限制，Mac打开终端SSH到群晖服务器（前提是你需要打开群晖的SSH功能）

![image](http://gtms01.alicdn.com/tps/i1/T1QysnFUJcXXb4zN_o-600-342.png)

注意使用root账户登录群晖，密码同你创建的admin账户

vi /etc/exports 将「insecure_locks」改成「insecure_locks」保存即可，如果你有多个NFS文件夹，应该会有多条记录，全部需要修改

![image](http://gtms03.alicdn.com/tps/i3/T15FFYFFhgXXa8Wwvo-600-140.png)

这个时候就可以使用XBMC正常访问到NAS里面的电影文件夹了：）

---

补充下安装群晖的DS Video应用后，使用iPad/iPhone观看视频也变得异常方便，对于非DTS音频（群晖没有版权）的高清视频（MKV封装）还可以做到实时转码，而且群晖默认自带TMdb刮削器，也能识别大部分电影信息（如果发现有电影信息不完全，还可以自己登录TMdb网站手工更新内容），值得一提的是试用DS Video必须将「电影」+「电视剧」分别扫描，因为TMdb对于这两类数据是分开存放的

![image](http://gtms01.alicdn.com/tps/i1/T1QfSoFJVeXXaRR5Po-600-434.png)

好了，这样基本全家的家庭影院就可以完没的无缝搭建，终于感觉到NAS对于提升家庭幸福感是多么重要的一个东西了：）

--EOF--