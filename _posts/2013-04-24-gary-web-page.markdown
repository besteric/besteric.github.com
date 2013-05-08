---
author: admin
comments: true
date: 2013-04-24 16:41:55
layout: post
title: 重大灾害网站变灰两三事
wordpress_id: 1013
categories:
- 蛋疼的事
---

上周六四川雅安地震了，虽然我目前为止也不清楚这个地方具体在哪，但是我们都第一时间接到上级通知，网站变灰以示哀悼。

**实现方案**

目前网络上最普遍的使用的以下CSS三部曲代码：

{% highlight css %}
html {filter: url("data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\'><filter id=\'grayscale\'><feColorMatrix type=\'matrix\' values=\'0.3333 0.3333 0.3333 0 0 0.3333 0.3333 0.3333 0 0 0.3333 0.3333 0.3333 0 0 0 0 0 1 0\'/></filter></svg>#grayscale") !important; /* Firefox 10+ */

filter:progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);

/*可以简写为：filter:gary*/

-webkit-filter: grayscale(1);
}
{% endhighlight %}

IE6-9 都支持[filter](http://msdn.microsoft.com/zh-cn/library/ms532853)属性，而webkit引擎浏览器也率先支持了[CSS Filter Effects 1.0](https://dvcs.w3.org/hg/FXTF/raw-file/tip/filters/index.html)，截至目前为止还是W3C的草案，[这个页面](http://caniuse.com/css-filters)详细分析了具体有那些浏览器支持-webkit-filter属性，最难搞的是Firefox 20+只能使用[SVG](http://www.w3school.com.cn/svg/svg_intro.asp)实现（[SVG Fliter支持浏览器列表](http://caniuse.com/#feat=svg-filters)）

**问题罗列**

1 [高富帅们的Retina MacbookPro抱怨图片和字体都看不清楚了](http://www.imququ.com/post/p20130420.html)，[据说这是一个webkit已知的Bug](https://bugs.webkit.org/show_bug.cgi?id=106241)，[解决方案也很简单](http://www.99css.com/archives/1329)，暂时没验证也没时间追查具体原因

2 [Safari下使用了背景色的区块简直惨不忍睹](http://img03.taobaocdn.com/tps/i3/T1N4WpXytXXXXpQ6Db-803-664.png)，暂时没想到特别好的Hack方案，当时采取了人肉设置问题区块的background-color：transparent；

3 使用上述三剑客CSS代码会导致Mac下Chrome滚动时随机出现黑线和黑块，具体原因不明

4 IE10悲剧的什么都不支持，[V2EX](http://www.v2ex.com/t/66465)上有同学提出可以使用Meta让IE10「优雅降级」为IE7/8/9，<meta http-equiv="X-UA-Compatible" content="IE=9" />

5 如果将三剑客作用于html，不管用各种优先级设置子区块为彩色显示都无法做到，推测可能和滤镜渲染的机制有关，暂时未深究（场景就是希望全页面都是灰色，但是某个特殊区块希望是彩色，醒目显示）

6 [Safari 5.1不支持-webkit-fliter属性](http://stackoverflow.com/questions/12685794/grayscale-image-with-css-on-safari-5-x)

根据[WPO](http://wpo.taobao.net/browser/)平台统计的访问淘宝浏览器数据分布来看，搞定trident和webkit引擎就可以覆盖98%以上的用户，所以SVG的Fliter可以考虑不加，因为对性能影响还是非常显著的。

![](http://img01.taobaocdn.com/tps/i1/T17R1oXplfXXaBMQr.-596-397.png)





最后请容许我发自内心的喊一句，WTF，请使用自己有限的时间到那些更有意义的地方上去吧...具体请访问[dowebsitesneedtolookexactlythesameineverybrowser?](http://dowebsitesneedtolookexactlythesameineverybrowser.com/)
