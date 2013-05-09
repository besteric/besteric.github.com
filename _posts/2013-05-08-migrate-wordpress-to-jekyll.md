---
layout: post
title: "Wordpress迁移至Jekyll"
categories:
- 偶尔惊喜
tags:
- wordpress
- jekyll


---

**写在前面**

2013年4月31日我一年前申请的[免费Amazon EC2主机](http://www.besteric.com/2012/05/03/Blog-migrate-to-amazon-ec2/)到期了，想来想去觉得续费也不划算，索性用phpmyadmin备份了数据库，FTP下载了Wordpress的所有文件，然后又将博客导出了XML文件，最后注销了EC2的帐号。

期间听同事说起[Oneasiahost](https://www.oneasiahost.com/)的服务器在新加坡，搭配[shadowsocks](https://github.com/clowwindy/shadowsocks)翻墙速度极快，而价格也非常便宜，每个季度只要12美刀。正好前段时间申请的招商银行AE金卡到手，我便刷了一个季度服务翻墙用，同时又觉得只跑个Node.js是不是太奢侈了点，琢磨着将Wordpress迁移到他的机器上。结果天生Linux手残到爆的我，只能拜托[@helloleo](http://twitter.com/helloleo)大神帮忙折腾了好几回，还是不得要领，机器经常内存跑满宕机，无奈只好作罢...想来想去还是使用静态博客Jekyll，迁移至伟大的Github上面吧：）

**迁移指南**

网上很多教程，我也搜了很多篇，推荐以下几篇文章

* [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
* [非常赞的Github指南——GotGitHub](http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html)
* [像黑客一样写博客——Jekyll入门](http://www.soimort.org/posts/101/)
* [使用jekyll来写博客的一些心得](http://webfrogs.me/2012/12/20/use-jekyll/)
本博客的Theme也是clone这个版本的

**积累的一些经验**

* 对Github还是不太熟悉，所以看了GotGitHub的文章后才开始迁移博客
* 最好还是现在本地大件Jekyll环境，调试OK后再Push到Github上来
* wordpress数据迁移，我使用的是[exitwp](https://github.com/thomasf/exitwp)，首先要升级Mac系统自带的Ruby版本，然后就是要安装他指定的几个模块，特别值得注意的是，安装完所有模块后执行转换命令一直提示找不到beautifulsoup这个模块的问题，[解决办法就是更换执行脚本的Beautifulsoup为bs4](http://www.crifan.com/python3_after_install_bs4_still_error_importerror_no_module_named_beautifulsoup/)
* 转换后的markdown文件名都是乱码，只能人肉一个一个修改了，囧
* 评论系统使用的是国内多说，UI界面看起来也还满意，就是今晚还发现他们一个大Bug，居然去除了Google账户的登录入口，导致我无法正常登录，后来找客服才临时解决这个问题。
* 这次迁移我把图片都抛弃了，看来以后都要遵循一个原则——鸡蛋不要放在一个篮子里

**推荐资源**

* 编辑器可以使用大名鼎鼎的Mou，也可以使用Mac上最好的编辑器Sublime Text 2
* Markdown语法速记可以参考[这篇文章](https://gitcafe.com/GitCafe/Help/blob/master/Markdown.md)
* 将Jekyll部署至Github上后，可以绑定独立域名，比如besteric.com，我的域名是在GoDaddy处购买的，但是域名解析却是使用的免费的CDN解析——[cloudflare](https://www.cloudflare.com)，修改CNAME和A不用5分钟就生效了，非常给力