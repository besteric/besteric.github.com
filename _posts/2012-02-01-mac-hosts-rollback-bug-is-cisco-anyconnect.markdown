---
author: admin
comments: true
date: 2012-02-01 14:26:42
layout: post
slug: mac%e4%b8%8bhosts%e9%87%8d%e5%86%99%e5%9b%9e%e6%bb%9a%ef%bc%8chosts-ac%e6%98%af%e7%bd%aa%e9%ad%81%e7%a5%b8%e9%a6%96%ef%bc%8ccisco-anyconnect%e4%bc%a4%e4%b8%8d%e8%b5%b7
title: Mac下hosts重写/回滚，hosts.ac是罪魁祸首，Cisco AnyConnect伤不起
wordpress_id: 884
categories:
- 偶尔惊喜
tags:
- anyconnect
- cisco
- hosts
- hosts.ac
- mac
---

写在前面，我尽量将标题写的对Google友好一点，希望能帮助到遇到类似问题的其他朋友！

2011年我将日常娱乐环境迁移至Mac下感觉良好，新年新气象，适逢淘宝新版首页开发索性将开发环境也迁移至Mac下，最近一直遇到一个很棘手的问题，修改/etc/hosts 文件后睡眠后或者重启后，即回滚至一个固定的版本，百思不得其解...最好搜索etc文件夹发现有一个可疑文件hosts.ac居然就是我那个阴魂不散的固定版本，更诡异的是修改hosts.ac文件后不就，hosts文件自动同步为最新版本。

Google 了下**hosts.ac**文件，发现这个居然是Mac下VPN客户端**Cisco AnyConnect**搞的鬼，安装AC后它会生成一份hosts.ac文件，当每次打开AC或者机器重启、睡眠后，都会触发hosts.ac文件替换hosts文件...

最终结论是安装了Cisco AnyConnect 的Mac用户如果需要修改hosts文件请vi /etc/hosts.ac  文件！

感谢@[PhartSmellah](http://ubuntuforums.org/member.php?s=9587637cc4a8f6283073259eaa6bcc91&u=1326834) 第一个在互联网上指出这个问题，[具体链接请访问这里](http://ubuntuforums.org/showthread.php?t=1896148)
