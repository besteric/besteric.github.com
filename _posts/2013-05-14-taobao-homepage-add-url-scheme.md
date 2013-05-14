---
layout: post
title: "淘宝首页增加URL Scheme"
categories:
- 蛋疼的事
tags:
- URL Scheme
- iPad
- 淘宝首页


---

这几天被iPad客户端运营追杀，要求在淘宝首页顶部加一个逻辑——iPad访问淘宝首页默认顶部会出现一个SmartBanner，用户点击也遵循以下逻辑

* 如果iPad上已经安装[淘宝HD应用](http://itunes.apple.com/cn/app/id438865278)，则默认打开[淘宝HD首页](http://itunes.apple.com/cn/app/id438865278)
* 否则跳转至iTunes提示用户安装最新版的应用

![image](http://img04.taobaocdn.com/tps/i4/T15JuGXqtcXXXl8iEP-1009-188.png)

----

如何检测iPad是否已经安装了[淘宝HD应用](http://itunes.apple.com/cn/app/id438865278)了？答案是使用URL Scheme，这里可以参考[Goagent](http://www.goagent.org/)的作者写的文章[iOS App 自定义 URL Scheme 设计](http://xujiwei.com/blog/2011/09/ios-app-custom-url-scheme-design/)

至于用户点击逻辑实现的思路就是：默认打开转淘宝HD应用，如果在500ms以内没有响应则认为机器未安装应用，页面重定向到iTunes应用下载页面

    <div id="J_Ipad_Notice"><a href="taobaohd://home" id="J_Ipad_Link" ><img src="$img" /></a></div>
    <script type="text/javascript">
    (function(){
     window.onload = function() {
        var ua = navigator.userAgent.toLowerCase();
        if(ua.match(/iPad/i)=="ipad") {
 
          document.getElementById('J_Ipad_Notice').style.display='block';
          document.getElementById('J_Ipad_Link').onclick=smartbanner();
          
          function smartbanner{          
            var startTime = +new Date;  
            setTimeout(function(){  
              if (+new Date - startTime < 500){  
                window.location = 'http://itunes.apple.com/cn/app/id438865278';  
              }  
            }, 500);   
          } 

        }
     };
    })
    </script>


----

Enjoy