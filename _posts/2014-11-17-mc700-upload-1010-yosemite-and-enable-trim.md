---
layout: post
title: "MC700升级到10.10 YOSEMITE并开启第三方SSD Trim"
description: 廉颇老矣，尚能饭否？
categories:
- 偶尔惊喜
tags:
- MC700
- MacbookPro
- upgrade
- SSD
- Trim


---

目前工作使用的Macbook还是11年3月购买的MC700，12年自己尝试升级硬件到了8G内存，[128G SSD + 320G HDD](http://besteric.com/2012/06/18/mc700-upgrade-samsung-128gssd/)，按照目前的配置来看，工作使用绰绰有余，但是使用Final Cut Pro编辑Gopro影片时还是有点捉襟见肘。对于13寸非Retina屏幕，倒是对我影响不大，因为大部分时间都是外接24寸显示器使用：）

前段时间Apple推出了10.10 Yosemite系统，我第一时间升级后发现三星830的SSD Trim无法正常开启了，当时比较懒，也就一直没开启。今天早上开机发现电脑SSD空间只剩下不到1G的时候，花了半小时清理文件，终于腾出了12G的空间，顺手还开启了SSD Trim功能。

开启依然很傻瓜化，下载[Trim Enabler](http://www.cindori.org/software/)即可，值得注意的是10.10对于安全验证有做改动

    In OS X 10.10 (Yosemite), Apple has introduced a new security requirement called kext signing. (A kext is a kernel extension, or a driver, in Mac OS X)

    Kext signing basically works by checking if all the drivers in the system are unaltered by a third party, or approved by Apple. If they have been modified, Yosemite will no longer load the driver. This is a means of enforcing security, but also a way for Apple to control what hardware that third party developers can release OS X support for.
    
所以使用Trim Enabler 3.3版本第一次enable会自动执行这条语句（需要管理员权限）

     sudo nvram boot-args="kext-dev-mode=1"
     
待机器重启后运行Trim Enabler重新enable，再重启一遍后，Trim自动打开

可以在「关于本机」-「系统报告」-「SATA/SATA Express」里面看到Trim是否开启

**重点需要注意的是**

<span style="color:red;">关闭系统验证 Kext 驱动程序的设置是存储在 Mac 的 NVRAM/PRAM 里面的。如果你手动清除了它们 (启动时按住 Cmd+Option+P+R)，那么非常可能会导致系统重新验证驱动程序从而无法开机。运行 Apple 的硬件维护程序也可能导致同样的状况。所以进行类似操作前一定要手动关闭 Trim 支持。如果你不幸中招了，覆盖方式重新安装系统是最快的解决办法（不会丢失个人文档）。</span>

**参考资料**

* [在 MAC OSX 10.10 YOSEMITE 下打开第三方 SSD 的 TRIM 支持](http://lipeng.de/blog/life/tech/994/)
* [FAQ and support for using Trim Enabler in OS X Yosemite](http://www.cindori.org/trim-enabler-and-yosemite/)