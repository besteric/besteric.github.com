---
layout: post
title: "Four ways to insert Flash to HTML"
categories:
- 
tags:
- Flash
- Object and Embed


---

好吧，今天看到有朋友问我博客为什么停止更新了，我本人为何放弃治疗了…拿「最近比较忙」搪塞下也不是个事，昨天正好遇到了一个Flash的问题就顺便记录下。

我本人对Flash没有什么好感，作为一名脑残果粉，每次打开有Flash的网页都能听到MBP的风扇在怒吼「你妹啊，又看小电影」，说起Flash的历史真是源远流长，虽然它有各种优势，但无法否认的一点就是Flash的生存空间已经被不断的挤压，延伸阅读：[Flash 真的是“落后的技术”吗？](http://www.zhihu.com/question/19851268) 

----

闲话扯完，回归到主题，最近忙着做淘宝香港首页，需求方觉得网页必须有个Flash广告才显得高端大气上档次…

关于Flash文件(SWF)在页面的引入还是有讲究的，我印象中有四种嵌入方式，不过每次需要的时候才去Google搜索，这次就当下搬运工:

原文由淘宝UED龙藏荣誉出品（淘宝曾经唯一的Flash工程师，现已转岗做PD）

先羞射给出四种「插入」方式的JS代码地址：[请猛戳我](http://a.tbcdn.cn/app/tms/others/flash_insert.js)

**OE Mode**

关于

* Adobe 官方式页面产出式
* 由 \<obejct> 和\<embed> 组合而成
* 默认由IDE产出，并有 \<noscript>包裹，并有js配合

优点

* 最强兼容性
* 遵从“优雅降级”方式
* 官方代码

缺点

* 最具差异性代码，不便于统一维护
* 无法自定义替换内容Alternative content
* 代码冗余。

其他

* 如果仅仅静态潜入。官方输出需要修改成为静态式
* 默认官方代码主要依靠JS动态加载。可能会受限于控制页面的权限

推荐场合

* 自己的站点，或不需要考虑太多问题的地方。因为官方都已做好，傻瓜式
* 需要最强兼容且不需要无法自定义替换内容Alternative content的地方

----


**OO mode**

关于

* SWObject 推荐的潜入方式
* 通过 IE条件注释 来区分 IE 或 非IE

优点

* PC全兼容
* 较少代码差异性，利于维护
* 可以自定义替换内容Alternative content

缺点

* 在某些浏览器下会多一次自定义替换内容Alternative content的请求。
* 在某些浏览器下多一次Flash请求，且不会从缓存取该内容。
* 代码较多

其他

* 有在线生成器 :)
* SWFOject生成动态代码都是Object。

推荐场合

* 所有可以在PC上观摩的页面。
* 当用户播放器可能没有安装或版本过低时，期望出现可替换内容的Alternative content
* 可能需要通过期望出现可替换内容的Alternative content面向SEO的

----

**LO mode**

关于

* 这个是基于 OO mode 的偷懒写法。
* 只有一个 <obejct>，以再次减少代码量

优点

* 兼容
* 代码差异性很小
* 代码较少
* 可以自定义替换内容Alternative content

缺点

* 和 OO mode 的相同
* 并多了一条缺陷：部分浏览器如Firefox 不会从缓存取该Flash内容。

推荐场合

* 对页面请求数要求不高的页面。
* 想偷懒又想能自定义替换内容Alternative content的页面。

----

**LE mode**

关于

* 仅有1个\<embed>
* 为了偷懒

优点

* 兼容
* 最少差异性
* 代码量最少
* 无多余请求

缺点

* 无法自定义替换内容Alternative content
* 非XHTML1.0规范
* 可能某些版本浏览器解析有问题。

其他

* HTML5 正式纳入规范

推荐场合

* 不需要自定义替换内容Alternative content的页面
* 想最少代码量。

----

顺便再提一下昨天遇到的的Flash遮挡DOM元素的问题，这篇文章已经图文并茂的解释的非常清楚了——[了解embed及param標籤中wmode屬性](http://josephj.com/entry.php?id=364)，实际上解决问题还是非常简单，设置wmode="opaque"，然后再设置对应的DOM元素的z-index高过Flash即可。