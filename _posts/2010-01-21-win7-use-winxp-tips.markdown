---
author: admin
comments: true
date: 2010-01-21 13:22:42
layout: post
slug: '%e5%9c%a8win7%e7%b3%bb%e7%bb%9f%e4%b8%8b%e5%ae%89%e8%a3%85%e7%8b%ac%e7%ab%8b%e7%9a%84xp%e7%b3%bb%e7%bb%9f%e4%bb%a5%e5%8f%8a%e5%88%a0%e9%99%a4%e5%a4%a7%e6%b3%95'
title: 在Win7系统下安装独立的XP系统以及删除大法
wordpress_id: 367
categories:
- 寂寞传说
---

本本刚买的时候就安装了Win 7，然后自己费了老大的劲捣鼓捣鼓又安装上了XP，但是发现用了Win 7后根本就不想再用XP了，而且MS系的我装一个系统就可以了，Linux的话我就装一个Ubuntu好了，网上搜了下如何在Win 7下删除 XP，MARK一下



> 
有不少朋友在安装Windows 7系统时，没有保留原来的XP操作系统，等到Windows 7用过一段时间后，才发现工作中某些办的在XP下运行更好。若是希望尝鲜新系统的同时又不影响正常的工作，需要考虑将Windows 7或XP双系统，而默认Windows 7环境下是无法直接装低版本的XP 系统。其实只要稍加改选也可以实现。
　
　　通过镜像恢复XP系统
　　 
　　如果大家以前安装过XP，可能会用Ghost备份过，可以用这个备份的XP系统的.gho系统镜像恢复到C盘以外的其他分区，在WIndows 7来来组成双系统。需要如下操作：

　　第一步：修改Ghost镜像安装分区位置。启动到Windows 7，运行GHost Explorer，打开之前备份好的Ghost镜像文件，本例为e:\Ghost目录下的Sys.gho。将该镜像文件根目录下的boot.ini、ntldr、ntdetect.com这三个系统文件提取到Windows 7系统所在的C盘的根目录下。接着打开文件夹选项的查看选项卡以显示所有具有隐藏属性的系统文件，再去掉系统文件boot.ini的只读属性，并用记事本打开，将文本中字符串Partition(1)中的1替换为2（2表示.gho格式的XP系统镜像将要将被恢复到D盘，若为3，则表示 E盘，依此类推），最后保存对系统文件boot.ini所做的修改即可。
　　 
　　第二步：添加XP菜单启动项。在Windows 7系统下，先安装配刊光盘附带的EasyBcd汉公版，运行后单击EasyBCD主界面左边的添加/删除（项目)按钮，然后单击版本右边的下拉箭头，选择Windows NT/2K/XP/2K3选项。接着在磁盘右边的文本框内输入c:\，在改名右边的文本框内输入自己喜欢的文字（c处所示的早期版本的Windows）。最后分辊单击界面中的添加和保存按钮，添加一个启动XP的菜单选项。
　 
　　第三步：用电脑迷光盘的WinPE光盘启动，恢复XP系统。运行WinPE系统下的无损分区软件WinPM7.0，接着右击win+r 7所在的C盘分区，选择隐藏，将该分区隐藏。再运行Ghost，将自己早已备份的XP镜像文件恢复到图2中D盘所在的分区。完成XP系统镜像的恢复操作后，启动WinPE并运行WinPm 7.0。最后再一次右击C盘分区，选择显现，显示已经隐藏的C盘分区即可。

　　经过以上操作，重新启动系统时，就可以看到那个很经典的双启动菜单了，选择其中的一个菜单项，就可以顺利地登录到XP或Windows7.在利用GHost镜像安装XP之前，隐藏Windows 7所在的C盘分区这一步必不可少，否则之前提前备份的Ghost版本的XP往往无法安装成功。 

    通过安装盘安装XP

　　若是之前没有XP的Ghost备份，又想在Windows 7系统下安装XP，使用XP的安装盘也行。

　　先将XP的安装盘放入光驱，接着执行安装XP的操作，将XP操作系统安装到C盘以外的任意一个分区。安装完毕后，暂时只能登录XP，这是由于 WIndows 7引导信息被XP安装程序覆盖。再根据上边第2步所介绍的方法，为XP一个启动菜单即可。完成上述操作后，登录XP并运行 EasyBcd，然后单击管理引导项目按钮，再勾选新出现的重新设置Windows Vista引导项目单选框并单击写入按钮即可。
　　 
　　在XP操作系统中运行Easybcd时，必须保证XP操作系统中已经提前安装了Microsoft .Net Framework 2.0环境。

　　PS：在双系统中如何删除XP系统
　
　　在XP+Windows 7双系统中删除XP也非常容易：在Windows 7系统下先格式化XP所在分区，然后显示所有具有隐藏属性的系统文件并删除C盘根目录下的Boot.ini、Ntldr、Ntdetect.com这三个系统文件，接着运行Easybcd，选中早期版本的 Windows，然后单击删除和保存按钮即可恢复Windows 7单系统。 




最后重启后发现启动栏还有“earlier version of windows”选项，我明明已经格式化了D盘，搜了下找到解决办法：管理员模式运行cmd
bcdedit /delete {ntldr} /f



