---
layout: post
title: "前端面试小结（HTML）"
description: 最近负责团队内部的前端招聘，每一次面试过程对于自己也是重新温习了一些知识点，有收获的时候就不免想沉淀记录下来：）
categories:
- 蛋疼的事
tags:
- 前端面试


---

最近负责团队内部的前端招聘，每一次面试过程对于自己也是重新温习了一些知识点，有收获的时候就不免想沉淀记录下来：）

**关于职责**

* 很多大型互联网公司（腾讯、携程）前端已经拆分为两部分——页面重构（美工）+JavaScript开发。前者负责页面设计和简单的HTML+CSS构筑，后者负责逻辑层面的JavaScript开发。

* 小型公司全能型，基本上从设计页面、切页面到具体的前后端实现都是一个人独立完成，擅长利用各种成熟的开源框架类库，快速实现指定功能。

* 阿里系的前端目前的趋势是「全端工程师」，对于Full Stack developer貌似有很多理解的方式，但是综合来看应该就是前后端+跨终端（HTML/CSS/JS+NodeJS+PHP...），只不过目前比例划分依然还是80%前端而已。


**一些问题**

**1 怎么看待HTML的语义化**

首先语义化不仅仅指的是HTML5那些新标签（header/footer...)，而且也包含使用\<div class="header">这种语义化Class的标签定义。

其次就是语义化带来的好处：

- 业界趋势发展，比如HTML5干掉XHTML 2成为业界新标准，而HTML5新增了大量语义化标签
- ~~手持移动设备的无障碍阅读~~(智能手机的发展已经完全可以渲染CSS)
- 对视障人士使用的读屏软件友好（屏幕阅读器对不同标签所发出的声音是不同的，使用更语义的标签以能传达不同信息的重要性）
- 搜搜索引擎友好，有助于SEO（未考证）
- 降低跨平台的成本

对于低版本的浏览器，为了让其支持HTML5新引入的语义化标签，可以使用[HTML5 Shiv](https://github.com/aFarkas/html5shiv)动态的将这些标签声明为Block元素（相当于DIV）

```html

<!--[if lt IE 9]>
    <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.js"></script>
<![endif]--> 
   
```

**2 doctype（文档类型）的作用是什么？**

\<!DOCTYPE>不是HTML标签，但是必须处于HTML文档的第一行，简单说来就是告诉浏览器应该使用什么协议来渲染页面，如果不指定则会引起很多怪异的问题（IE的quirks mode）。

因为HTML 4.01是基于[SGML](http://zh.wikipedia.org/wiki/SGML)，所以需要使用[DTD](http://zh.wikipedia.org/wiki/DTD)

```html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

```

HTML5不基于SGML，所以声明变得异常简单，全浏览器兼容

```html

<!DOCTYPE html>

```

其他的参数选择可以参考下[w3school](http://www.w3school.com.cn/tags/tag_doctype.asp)里面的定义，再此不在赘述。

**3 GET 和 POST 的区别?**

这个问题如果从字面来比较，可以参考[W3C](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)的定义

如果仔细深究下两者的区别可以参考下这两篇文章：

* [从HTTP GET和POST的区别说起](http://www.yining.org/2010/05/04/http-get-vs-post-and-thoughts/)
* [HTTP POST GET 本质区别详解](http://blog.csdn.net/gideal_wang/article/details/4316691)

**4 cookies，userdata,sessionStorage 和 localStorage 的区别**

* [cookies](http://en.wikipedia.org/wiki/HTTP_cookie)最大的特点是全浏览器兼容，但是容量相对最小（一般为4KB，且服务器保存总数不超过20个，浏览器保存总数不超过300个），并且每次http请求都会发送cookie至服务器（增加额外的带宽开销），因此也容易带来一些安全隐患。
* [userdata](http://msdn.microsoft.com/en-us/library/ms531424(v=vs.85).aspx)是IE特有的存储格式(最大支持64KB,域名下不超过640KB)
* sessionStorage + localStorage 统称为[Web Storeage](https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/DOM/Storage/Storage)，是HTML5的新特性（事实上，IE8+与高级浏览器都支持），两者主要区别在于有效期，SS提出了窗口的概念（Tab）；LS基本上不手动清除会一直存在；两者容量最大值都是8MB，是目前最大容量的存储格式，并且拥有原生的set/get/remove/clear方法进行增删改操作。

综合来看，如果要考虑兼容性的化，可以使用Web Storage+UserData封装一个函数，或者直接使用开源JS缓存框架，比如[LocacheJS](http://locachejs.org/)


---


**写在后面**

说实话，HTML其实人人都会，如果深究也还是能挖掘出一些有意思的东西。如果以全职JS工程师自居，反而忘记了很多HTML基础知识，恐怕只会将自己的职业规划局限在一个很狭小的空间吧？

下一篇我会整理一些常见的CSS问题。

--EOF--







