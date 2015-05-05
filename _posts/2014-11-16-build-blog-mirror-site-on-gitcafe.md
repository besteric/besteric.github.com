---
layout: post
title: "博客迁移回国内的Gitcafe Page"
description:
categories:
- 偶尔惊喜
tags:
- github
- gitcafe
- jekyll


---

去年上半年从Wordpress迁移至Jekyll，觉得使用Markdown享受单纯的码字非常快乐，但是最近国内局域网封锁的太厉害，访问Github也慢到吐血，每次push的时候都要等待10多秒，实在是苦不堪言...

Google了下发现国内的Gitcafe也支持Pages部署静态博客了，而且操作非常简单，花了半小时就折腾好了，顺便将域名解析也指向了Gitcafe的镜像站，So easy.

---

整体思路就是将之前的Github仓库增加一个[remote],这样就可以将同一份代码同时提交到github和gitcafe上（双重备份比较保险，国内的大环境，你懂的）

    vi .git/config
    
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
        ignorecase = true
    [remote "origin"]
        url = https://github.com/besteric/        besteric.github.io.git
        url = git@gitcafe.com/besteric/besteric.git
        fetch = +refs/heads/*:refs/remotes/origin/*
    [remote "gitcafe"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = git@gitcafe.com:besteric/besteric.git
    [remote "github"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = https://github.com/besteric/besteric.github.io.git
    [branch "master"]
        remote = origin
        merge = refs/heads/master
        

修改以后使用git push github/gitcafe 即可提交到指定的仓库，执行git push 则会自动提交到两个仓库

这个时候出现一个问题就是git push是提交master分支的，这在github没什么问题，但是gitcafe目前只支持gitcafe-pages分支设立静态Blog，所以需要手动执行下 git push gitcafe master:gitcafe-pages (本地分支与远程分支不一致导致)

然后去域名管理后台修改A记录指向117.79.146.98 即可享受国内网络秒开blog的快感了：）

---

2015-05-05 更新

可以在 .git/config 中添加一个 alias 来实现同时提交gitcafe和github

    [alias]
        publish = !sh -c \"git push gitcafe master:gitcafe-pages && git push github master &&\"


这样每次执行 git publish就可以将文件同步到两个仓库了

---

参考资料：

* [将博客从GitHub迁移到GitCafe](http://blog.devtang.com/blog/2014/06/02/use-gitcafe-to-host-blog/)
* [git同步github和gitcafe](http://ikehr.com/%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7/2014/05/02/she-zhi-git-tong-bu-github-he-gitcafe/)
* [同时使用 GitHub 与 GitCafe 托管博客](https://ruby-china.org/topics/18084)
* [同步github上的项目到gitcafe](http://cxh.me/2014/06/28/gitsync-github-gitcafe/)