---
author: admin
comments: true
date: 2010-02-08 17:35:36
layout: post
slug: selenium%e5%a4%84%e7%90%86%e5%bc%b9%e5%87%ba%e7%aa%97%e5%8f%a3
title: Selenium处理弹出窗口
wordpress_id: 417
categories:
- Selenium
tags:
- Pop-ups
- Selenium
- 弹出窗口
---

原帖出自：[http://fafeng.blogbus.com/logs/56326780.html](http://fafeng.blogbus.com/logs/56326780.html)

在51testing看到的链接，今天没什么精神觉得总结的不错，先转过来再说

对网页弹出窗口，如[WIKI所述](http://wiki.openqa.org/display/SEL/Selenium+Core+FAQ#SeleniumCoreFAQ-HowdoIworkwithapopupwindow%3F)，若要保持脚本运行稳定，必须在waitForPopUp这个弹出窗口之后紧跟运行selectWindow命令选中这个弹出窗口（[示例](http://qtp-help.blogspot.com/2009/07/selenium-handling-windows.html)），如果仍不稳定请参考[这个示例](http://stackoverflow.com/questions/99045/handling-browser-pop-up-windows-with-selenium)。[这里](http://epyramid.wordpress.com/2009/04/23/selenium-rc-and-pop-up-handling/)介绍了chooseCancelOnNextConfirmation、chooseOkOnNextConfirmation等JavaScript脚本实现的弹出窗口处理函数，selenium并不会弹出窗口，因为它重写了window.open在文件selenium-browserbot.js函数BrowserBot.prototype.modifyWindowToRecordPopUpDialogs中的newOpen，但这必须在window.onload之后创建才有效。对于HTTPS安全性弹出窗口证书的处理，见[Selenium RC](http://seleniumhq.org/docs/05_selenium_rc.html#handling-https-and-security-popups)。

对非网页弹出窗口，如window.alert，window.confirm，window.prompt，window. showModalDialog等，有如下方法：

1.封装Windows Api，对Java语言则有Java Native Interface ([JNI](http://java.sun.com/docs/books/jni/))或者[J/Invoke](http://www.jinvoke.com/)（[示例](http://www.jsystemtest.org/?q=node/70)）。

2.Selenium RC中开启proxy injection(PI)模式也可以识别，这种模式提供了一个HTTP代理在浏览器之前自动更改所有接收到的HTML。window.alert, window.confirm,window.prompt在文件selenium-browserbot.js函数BrowserBot.prototype.modifyWindowToRecordPopUpDialogs中被覆写。

3.[这里](http://www.javaeye.com/topic/434092)用window.open覆写了window. showModalDialog，同样实现的还有在文件selenium-browserbot.js函数BrowserBot.prototype._modifyWindow实现开始部分添加对ModalDialog的[实现](http://seleniumdeal.blogspot.com/2009/09/changing-selenium-jar-to-globally.html)。
