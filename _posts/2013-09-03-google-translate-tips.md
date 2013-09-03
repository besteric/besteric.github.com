---
layout: post
title: "如何在Google Translate翻译基础上二次翻译"
categories:
- 偶尔惊喜
tags:
- Javascript轮询
- setTimeout
- Google Translate


---

**背景**

最近在做淘宝新加坡马来西亚站点，有一个需求是利用Google Translate提供多语种版本，目前Google Translate提供两种调用方式：

* [网站翻译器](http://translate.google.com/manager/website/suggestions)
    * 优点——傻瓜化调用，直接在head加入meta，然后随便找个地方插入一小段脚本即可对全站进行翻译
    * 缺点——太傻瓜，除了可以定制翻译的语种，几乎无法做其他修改
* [Google Translate API](https://developers.google.com/translate/)
    * 可定制化翻译方案，更加灵活
    * 付费，而且价格不便宜，按次数收费...

**问题**

基于成本考虑，我们选择使用最基础的「网站翻译器」，但是问题随即而来。网站为了保持所有浏览器显示效果一致，用了设计师的大招——所有标题的背景都是图片，**Google Translate目前还无法翻译图片** T_T

**思路**

Google Translate解决不了的问题，我们自己解决：）但是前提条件是必须知道用户目前翻译的是何种语言，这样我们才能对症下药，由于Google Translate的语言选择框是一个iFrame，我们无法监测到用户的操作，所以只能猥琐的在页面添加一个隐藏的元素，然后用JS轮询访问这个元素是否被翻译成目标语言，如果翻译了，则用JS给Body加上对应的语言Class，后续的就是纯CSS体力活了
        
      <!--隐藏的DOM元素-->
      <div id="hello" style="display:none">宝贝</div>
      
      <script>
      var S = KISSY;
      var D = S.DOM;
      var before = D.text('#hello');

      function checkhello(){
      var now = D.text('#hello');
      if(now == before){
          //donothing
          setTimeout(checkhello,1000);       
      }
      else{      
          //clear before class of body
          D.removeAttr('body','class');
          
          switch(now){
              case 'Baby' : 
                  console.log('英文');
                  D.addClass('body','eng');
                  break;
              case '寶貝' :
                  console.log('繁体');
                  D.addClass('body','cht');
                  break;
              case '宝贝' :
                  console.log('中文');
                  D.addClass('body','chs');
                  break;
              default :
                  console.log('其他');
                  D.addClass('body','eng');
          }
          
          before = now;
          setTimeout(checkhello,1000);      
      }
      }
      checkhello();
      </script>


--EOF--