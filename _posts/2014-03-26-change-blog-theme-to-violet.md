---
layout: post
title: "更新博客主题"
description: 春天来了，又到了万物复苏的季节，使用了快1年的Jekyll，原来的主题怎么看怎么变扭，于是花了一些时间更换了主题
categories:
- 偶尔惊喜
tags:
- jekyll
- theme
- violet
- migrate


---

去年将[Wordpress迁移至Jekyll](http://www.besteric.com/2013/05/08/migrate-wordpress-to-jekyll/)，使用了差不多1年的[极简主题](http://webfrogs.me/2012/12/20/use-jekyll/)，最近怎么看主题怎么不得劲，寻思得换个风格换个风情了...

去[Jekyll Theme](jekyllthemes.org)挨个看了一圈也没有找到钟意的，无意访问了展新的Blog才发现有几款Jekyll的主题都不错，最终锁定了目前博客使用的主题——[violet 2](http://www.zhanxin.info/jekyll/2013-10-29-new-violet-theme.html)，得益于Jekyll简单明了的目录结构，换主题无非是将_post文件夹下所有的日志markdown复制到新下载的工程里面，然后针对自己的个性需求，再适当修改下对应的JS/CSS以及_config.yml文件

PS：关于_config.yml可以参考下它的[官方网页](http://jekyllrb.com/docs/configuration/)，以及它使用的YAML语法规则，可以[参考这里](http://jekyllrb.com/docs/frontmatter/)

比如我的_config.yml部分代码如下：

```yaml

markdown: redcarpet
pygments: true
plugins: ./_plugins
paginate: 8
permalink: /:year/:month/:day/:title/

safe: false

title: Pretend To Write Like A Hacker
subTitle: besteric.
description: 记录生活点滴，积累沉淀...
url: http://besteric.github.com
feed: /atom.xml

author:
  name: besteric
  link: http://www.besteric.com/contact.html

```

在其他地方如果需要引用\_config.yml配置的name属性，可以使用这种形式：\{\{ site.author.name \}\}，其实简单理解将\_config.yml赋值给site这个变量，相对应的变量还有一个\{\{page\}\}，也是指代了每一个页面的属性，具体的语法规则可以看[这里](http://jekyllrb.com/docs/configuration/)

在更新完主题后，我顺手更新了下本地的Jekyll，直接大跃进至1.5.0版本，为了使用最新的[Github Flavored Markdown](https://help.github.com/articles/github-flavored-markdown)语法，以及使用Fenced code blocks高亮代码块模式，我毫不犹豫的将markdown渲染引擎从rdiscount修改为redcarpet

这样就可以使用Fenced code blocks(中文名貌似叫「围栏」)方式高亮各种代码了

    \\\javascript

    function HelloWorld(){
        console.log('Say hello to redcarpet')
    }

    \\\

```javascript

function HelloWorld(){
    console.log('Say hello to redcarpet')
}
```


**共享主题**

violet 2遵循[The MIT License](http://opensource.org/licenses/MIT),所以有需要的同学可以去下载原版使用，或者直接拷贝我的[Github](https://github.com/besteric/besteric.github.com)

**参考资料**

* [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
* [Markdown 渲染引擎折腾记](http://www.douban.com/note/147668553/)
* [jekyll + markdown使用笔记](http://blog.coloam.com/jekyll/markdown/2013/09/22/markdown-note/)
* [让Jekyll将Pandoc作为Markdown的渲染器](http://dannpu.com/blog/2013/08/02/new-post/)
* [用Jekyll和Pygments配置代码高亮](http://zyzhang.github.io/blog/2012/08/31/highlight-with-Jekyll-and-Pygments/)


-EOF-
