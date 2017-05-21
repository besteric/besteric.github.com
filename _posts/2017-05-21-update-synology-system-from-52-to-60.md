---
layout: post
title: "群晖系统从 5.2 升级至 6.02 的坎坷经历"
description: 最近为了折腾智能家居，需要在 docker 上运行 homebridge 和 home assistant 这些框架，而老版本的群晖对 docker 支持的并不理想，只能客服懒癌，将家里万年不升级的群晖系统升级到了6.02...
categories:
- 蛋疼的事
tags:
- 群晖
- NAS
- synology


---

### 为什么要升级

其实我在几天前的群晖系统还是 5.1 的版本，因为足够稳定并且能满足我的所有需求，所以一直没有折腾它。但是最近突然对智能家居有点兴趣，特别是小米全家桶又能通过 Homebridge 接入苹果的 homekit 里面，而 Homebridge 是可以运行于任何 linux 环境。家里 7x24 小时开机的 linux 环境除了 Netgear R6900（刷了梅林固件），就是群晖系统了，考虑到稳定性，将 Homebridge 放在群晖的 docker 下运行最合适不过...

那么问题来了，我目前使用的 5.1 系统还是2015年发布的，并不支持 docker 这个套件，最起码也要升级到 5.2 的版本

### 5.1升级到5.2

这个升级还是和以往一样，遵循以下几个步骤即可顺利升级：

1. 下载5.2对应的引导文件（XPEnoboot img），并用 OSFMount / UltraIso 修改引导文件的 /grub/grub.cfg 中的 pid, vid, sn, mac 地址，其中 sn/mac 如果不追求洗白的效果可以不改（我就没改），如果你想洗白可以用 [xpenology 提供的算号器](https://xpenology.github.io/serial_generator/serial_generator_new.html)。

Synoboot支持的U盘VID和PID如下：

    * VID=F400, PID=F401 (Synology)
    * VID=090C, PID=1000 (慧荣主控)
    * VID=0781, PID=5571 (闪迪酷豆)
    * VID=0EA0, PID=2168 (瀚邦主控)

推荐以上U盘，可以修改XPEnoboot的img，隐藏U盘

2. 使用 win32diskimager 将修改好的 img 写入启动U盘，将其插入群晖（最好选择2.0的USB口）
3. 设置 BIOS 启动项为 U盘 启动，开机
4. 选择 Install/Upgrade 选项从原版本带数据无损升级至最新系统
5. 使用浏览器访问 http://find.synology.com 或者使用 Synology assistant 查找局域网的群晖服务器，并手动上传对应系统的 .pat 文件
6. 等待安装和自动重启
7. 完成

整个流程对我最大的挑战就是...第一步、第二步修改 grub.cfg 配置文件和将 img 写入U盘的软件，在Mac下支持的并不好，所以每次还得开个 windows 的虚拟机完成操作，蛋疼啊...

![](https://img.alicdn.com/tfs/TB1ehAZRXXXXXawXVXXXXXXXXXX-665-444.jpg)

### 5.2升级到6.02

本来5.2已经支持 docker 了，照理说可以不用再升级到最新的 6.0 系统了，但是在使用 docker 的过程中发现这个版本有些兼容性问题，特别是 homebridge 在这个版本的 docker 下运行会不定时报错，痛定思痛，还是得升级到 6.0 版本

参考了这篇文章 [Tutorial: Install/Migrate DSM 5.2 to 6.0 (Jun's loader)](http://xpenology.com/forum/viewtopic.php?f=2&t=22100) 升级过程和 5.2 出入不是特别大，需要注意的几个点：

1. 6.0版本的引导文件（synoboot.img loader）和 5.x 时代（XPEnoboot img loader）完全不一样，所以对主板配置有一些特殊的要求，比如 `HDDs are booting in AHCI mode and not in IDE`
2. 如果是 Intel 芯片的主板，刷好U盘插上群晖，重启后完全不用键盘操作了，自动进入 `Booting the kernel` 状态，使用浏览器访问 http://find.synology.com 或者使用 Synology assistant 即可找到待升级的服务器了
3. 这个引导（ v1.01 ），使用群晖系统自带的升级功能，最多只能升级到 `DSM 6.0.2-8451 update 11` ,但是不能升级到 6.0.3, 6.1 or 6.1.1

这个过程对我困扰最大的是——索泰 N230 主板网卡驱动并不在默认的 synoboot.img （v1.01）版本里面，导致我在进入 `Booting the kernel` 状态时完全找不到群晖服务器，反复折腾了几次都失败，不得不退回到 5.2 系统

![](https://img.alicdn.com/tfs/TB1Tyo4RXXXXXbpXFXXXXXXXXXX-720-400.jpg)

在国内找了下类似的案例，以及 XPEnoboot 官方给出的指南都指出了驱动的问题：

> If after following the tutorial you can’t find your NAS through http://find.synology.com ou Synology Assistant it is highly possible that the drivers of your NIC are not included in the ramdisk of the loader. In doubt, just ask.

就当我快绝望的时候，并在 XPEnoboot 咨询了版主能否将缺失的驱动整合进 jun's loader v1.01，想不到柳暗花明又一村，版主早已考虑到我们这类用户，从 quicknick loader 提取了另外一个驱动集合文件 `Extra.lzma` ，直接替换原有 img 目录下的同名文件即可,原文如下：

> This custom ramdisk is optional but I recommend it. I am just providing it for those who are having issues with network detection or unrecognised HDD controllers. This custom ramdisk contains additional modules (drivers) that were mostly taken from Quicknick's loader. I don't warranty they all work but I think most do. You will need to replace (or rename, so you can revert) the default extra.lzma ramdisk from Jun's loader with this one. See change log at the end of the tutorial for additional modules.

替换 [Extra.lzma](https://mega.nz/#!HRJQGbhA!moPPC7fRCn-4fl6MirmYUmeAb2KzmxM5DE8SaazHDg8) 后升级就是一帆风顺的点点鼠标啦~

### 6.02 重大升级

升级到 6.02 后发现群晖 Web 登录页面变了，几乎所有套件都有重大升级，当然我最关心的 Docker 套件也升级到了最新版本，试了下系统原有的配置都没问题，Finally，可以放心折腾智能家居这个坑了...

貌似 XPEnoboot 对于 6.1 版本的引导文件还在调优（对于 AMD 芯片的主板支持不太友好），我应该短时间不会考虑将系统再做升级了，毕竟服务器的稳定性高于一切：）

![](https://img.alicdn.com/tfs/TB1SMQVRXXXXXbeXVXXXXXXXXXX-978-602.png)

![](https://img.alicdn.com/tfs/TB1XUVdRpXXXXcCXpXXXXXXXXXX-977-598.png)

![](https://img.alicdn.com/tfs/TB1Wc3FRXXXXXaXapXXXXXXXXXX-980-599.png)
