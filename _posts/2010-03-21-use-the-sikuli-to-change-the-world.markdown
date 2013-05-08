---
author: admin
comments: true
date: 2010-03-21 15:46:43
layout: post
slug: use-the-sikuli-to-change-the-world
title: Use the Sikuli to change the world
wordpress_id: 494
categories:
- 爱撕寂寞
tags:
- Sikuli
---

记得看过一部电影《上帝之城》给我留下了深刻的印象——无法超越的高度，种族纷争，让人沉思

年初的时候MIT的一位博士放出来一个图像化编程语言Sikuli（**在墨西哥维乔印第安人(Huichol Indians)的语言里是上帝之眼的意思**）的demo。迅速的引起了世界的关注，当时由于春节我也只是在Youtube上面简单的看过这个视频，Sikuli的工作模式与人眼一样，直接识别图像，而不是底层代码，因此不会产生不兼容的问题。

值得一提的是Sikuli的主创人员之一是来自台湾的张琮翔（[猛击这里围观](http://blog.vgod.tw/2010/01/25/change-the-world/)）

Sikuli的官方主页已经提供了 Windows／Mac／Linux 平台下的IDE，今天正好有一些时间索性在Snow Leopard下面写了个简单的”Hello World“体验下世界上最新的研究成果，发现目前的IDE功能还是非常简单的，期待作者在正式版中更新:)

以下是[Seven](http://www.51testing.com/?uid-159438-action-viewspace-itemid-208125)同学的评论：


> 美国理工学院学生的一个杰作，非常有意思。比较符合人类的思维。
具体的例子可以去官方的文档。
表面上看，是采用图像判断的方式去执行。
支持**测试**使用。不过测试比较简单。

使用了一下，发现不错。
可以进行重新的封装与改造。用来做测试，是非常优秀的。

它的工具的亮点，就是模拟了人是思维。
目前GUI自动化的最大缺点，就是按照机器思维，而不是人的思维去实现。
结果就导致了公司里面的自动化比较难以应对变化。
UED的修改，对测试造成了很大的影响，给重用带来了不小的阻力。

改进的方式，除了测试影响研发与UED外，还可以通过完善脚本来实现。
其实，自动化，不要专注于某个自动化对象的name，或者id。
而更应该关注的是对象的对人类可见的属性，比如带什么名字的按钮，什么颜色的按钮等。
这样可以让传统的自动化脚本，可以更加的应对GUI变化。

而sikuli，正是基于这个思路。

不过sikuli的缺点也是很明显的。纯粹的图像比对是不能解决问题的，如果某个按钮的字体没有改变，但是颜色改变了，那么自动化也会是个问题。
更好的GUI自动化，应该是针对人类视觉的模拟+传统方式的识别。

sikuli的测试断言功能目前比较弱，只有

>     
>     assertExist与assertNotExist
>     
>     虽然目前它不够完善，管理机制不够健全，离项目应用还有很大的差距。
>     但是值得一试。。。。
> 
> 



[![](http://www.besteric.com/wp-content/uploads/2010/03/sikuli.jpg)](http://www.besteric.com/wp-content/uploads/2010/03/sikuli.jpg)
