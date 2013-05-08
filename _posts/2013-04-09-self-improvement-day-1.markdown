---
author: admin
comments: true
date: 2013-04-09 16:39:57
layout: post
slug: '%e4%b8%83%e6%97%a5%e6%8f%90%e9%ab%98%e8%ae%a1%e5%88%92-day-1'
title: 七日提高计划——Day 1
wordpress_id: 1000
categories:
- 蛋疼的事
---

很遗憾的说一句，没有做到0点前睡觉这一「生活计划」，因为我意识到以目前的学习状态来看，效率太低，只能拖长时间（其实是借口）

**今日阅读**

[Google的HTML/CSS风格指南](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)

总体部分

1 资源引用中http/https 可以忽略

2 缩进建议使用两个空格代替Tab

3 全部小写

html部分

4 按照目的使用HTML标签，比如包含Click事件的元素尽量使用a而不是div

5 图片和视频/音频资源建议使用Alt属性描述，增加无障碍访问，以及可读性

6 <html><head><body>标签都可以省略，W3C也认可这种做法，但是从代码阅读层面考虑还是不建议这么操作。

7<link><scrpt>标签如果仅仅是引用css和JS，可以省略type属性

css部分

8 命名长短合适，class命名语义化，避免使用位置，颜色等特征

9 使用简短的seletors，避免使用*等通配符，也避免使用元素选择器(div.hello之类)因为CSS的解析是从右至左（从底至上）

10 合并属性，比如margin，padding，border等

11 0后面不用接单位（px）

12 16进制颜色可以简写，比如#ffeeff == #fef ，但是在HTML中却不能简写

13 空格的使用

seletor {

border: 1px;

}

[为什么推荐两个空格代替Tab作为代码缩进](http://www.zhihu.com/question/19960028)

让我记住的观点是Tab在各种环境下也许不一致，但是空格就没这个问题。

[中国地下互联网世界的冰山一角](http://www.techread.cn/?p=333)

权当互联网八卦来看了，黑色产业链确实不容忽视，包括文中没有提及的垃圾广告流量站以及SPAM产业链

[做个懂产品的程序员](http://www.luanxiang.org/blog/archives/1469.html)

很遗憾我去年做了大半年的产品角色，最后觉得缺少技术沉淀辅助决策，今年开始放弃产品深入的路线，还是集中精力做技术，套用一句经典的话「抬头看路，低头赶路」

**Mac软件**

[CoRD](http://cord.sourceforge.net/)代替MS的Remote，据说可以解决内存泄漏的Bug

[Flux](http://stereopsis.com/flux/)保护视力，目前使用上感觉比较新奇，具体效果看情况而定

**灵光一现**

1 今天处理了很多加入「原单购物群」的申请，加上每天都会去刷@什么值得买 的信息，发现人们的购买欲望还是非常强烈的，考虑和同事一起做一个淘原单的微信App或者Blog

2 性能优化虚拟小组，每个礼拜会总结本周的一个进展分享给组内成员

最后给自己今天的提升系数打分的话，我觉得只能获得3分（满分为10分），目标不明确，太杂太乱！
