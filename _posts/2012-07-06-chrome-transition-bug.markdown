---
author: admin
comments: true
date: 2012-07-06 08:27:46
layout: post
slug: '%e5%86%8d%e8%af%b4%e4%b8%8bchrome%e6%b8%b2%e6%9f%93transition%e7%9a%84%e5%90%84%e7%a7%8dbug'
title: 再说下Chrome渲染Transition的各种Bug
wordpress_id: 935
categories:
- 蛋疼的事
---

**问题重现:**

大致在半年前发现[Chrome渲染Transition时页面闪动Bug](http://ued.taobao.com/blog/2012/01/06/chrome%E6%B8%B2%E6%9F%93transition%E6%97%B6%E9%A1%B5%E9%9D%A2%E9%97%AA%E5%8A%A8bug/)，最近在Mac下的Chrome 20.0.1132.47版本发现这货又死灰复燃了，更离谱的是Hover淘宝首页吊顶的其他选项时，购物车会消失...有兴趣的同学可以访问这个[代码片段](http://jsfiddle.net/besteric/BCZUx/4/)重现下

![](http://img04.taobaocdn.com/tps/i4/T1oTzlXcBtXXXXY4Zf-550-25.png)

![](http://img03.taobaocdn.com/tps/i3/T1umDrXldfXXbFQn3g-554-29.png)

**事故原因:**

虽然我们可以合理推断这个问题依然还是Chrome那糟糕的动画渲染功能，但是更进一步的思考下为什么只有购物车消失了？经过简单的排查后发现是这行代码导致的：

#site-nav .quick-menu .mini-cart #mc-menu-hd {overflow: hidden;}

**如何解决：**

添加 -webkit-backface-visibility: hidden; 到出现问题的DOM父节点，可以避免购物车消失的问题。

**新的问题：**

Mac下会出现使用Transition的DOM结构字体变得非常浅，Windows下没有此问题...

![](http://img04.taobaocdn.com/tps/i4/T1APbqXdxqXXa.l.3i-563-28.png)

**不是最佳的解决方案：**

放弃Chrome下使用Transition属性，删除 -webkit-transition: -webkit-transform .2s ease-in; 世界再次美好了！

**参考资料：**



	
  * [http://stackoverflow.com/questions/5078186/webkit-transform-rotate-pixelated-images-in-chrome](http://stackoverflow.com/questions/5078186/webkit-transform-rotate-pixelated-images-in-chrome)

	
  * [http://code.google.com/p/chromium/issues/detail?id=36902](http://code.google.com/p/chromium/issues/detail?id=36902)

	
  * [http://stackoverflow.com/questions/6492027/css-transform-jagged-edges-in-chrome](http://stackoverflow.com/questions/6492027/css-transform-jagged-edges-in-chrome)



