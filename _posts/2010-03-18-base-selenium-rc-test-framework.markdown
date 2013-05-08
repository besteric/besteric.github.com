---
author: admin
comments: true
date: 2010-03-18 02:10:05
layout: post
slug: '%e5%9f%ba%e4%ba%8eselenium-rc%e7%9a%84%e8%87%aa%e5%8a%a8%e5%8c%96%e6%b5%8b%e8%af%95%e6%a1%86%e6%9e%b6'
title: 基于Selenium RC的自动化测试框架
wordpress_id: 453
categories:
- Selenium
tags:
- Ant
- Hudson
- Selenium
- SVN
- TestNG
---

首先要说明的是这个自动化测试框架搭建纯属个人爱好，欢迎指出不足的地方

**简单介绍**

[Selenium](http://seleniumhq.org/):这个不解释，不懂得也别往下看了

[TestNG](http://testng.org/doc/index.html): 是一个设计用来简化广泛的测试需求的测试框架，从单元测试到集成测试,基于Java语言的

[Ant](http://ant.apache.org/): 很多人应该都知道。一个项目里，每次修改一个细节，都需要重新编译，打包，测试，等等一系列过程，复杂又重复。Ant就是把这个过程给自动化掉了。在这里会用到的就是 自动编译，自动启动Selenium server，自动运行测试，自动关闭Selenium server

[SVN](http://subversion.tigris.org/): 版本控制工具

[Hudson](https://hudson.dev.java.net/): 自动地按照你设计的时间表，从SVN里下载最新的代码，然后自动调用Ant来完成自动编译，自动运行测试。然后生成结果文件发送报告等等

**搭建过程**

**安装Hudson**

从官网下载最新的Hudson安装文件（[请猛击我](http://hudson-ci.org/latest/hudson.war)），确保系统已安装了JDK和.NET Framework

1.在命令行窗口运行** java -jar hudson.war**（先定位到文件夹）默认端口是8080，如果此端口已被占用，建议使用** java -jar Hudson.war --httpPort=8088**(随便你用什么端口，别重复就OK)

2.在浏览器地址栏输入 **http://localhost:8080**（或者你安装的端口）访问，建议将Hudson安装作为Windows Service，这样就不用每次都进入命令行窗口敲命令了。选择左边的**Manage Hudson**，然后**Install as Windows Service**，选择好你的安装目录（以后项目代码的存放位置）开始安装

**架设SVN服务器**

版本控制工具我想不用介绍如何使用了吧？如何在本地搭建一个ApacheSVN的话我推荐安装[印第安童鞋](http://indian.blog.163.com/blog/static/10881582007112415021751/)的教程设置，这里我就不赘述了。因为我自己在实际使用中更倾向于使用Google Code的SVN服务，主要是设置简单使用方便:)

**准备测试用例**

简单写了一个Google关键字“besteric”的测试用例，通过Build.xml执行测试用例

**TestGoogle.java**
`
import org.testng.Assert;
import org.testng.annotations.*;
import com.thoughtworks.selenium.DefaultSelenium;
import com.thoughtworks.selenium.Selenium;
public class TestGoogle {
public Selenium selenium;
@AfterClass
public void tearDown() {
//After test closed the Firefox Browser
 selenium.stop();
}
@Test
public void TestGoogle() {
String url = "http://www.google.com";
//4444 is default server port
selenium = new DefaultSelenium("localhost", 4444, "*chrome", url);
selenium.start();
selenium.windowMaximize();
//open QA site
selenium.open(url);
selenium.type("q", "besteric");
selenium.click("btnG");
selenium.waitForPageToLoad("30000");
Assert.assertEquals("besteric - Google Search", selenium.getTitle());
}
}`

**Ant.xml (TestNG的suite文件) **

**
**

`


  
    
              
    
     

`

**Build.xml (Ant文件要放在project根目录)**

**
**

`
  
 	  
 	  
 	  
 	  
      
     
 	
         
         
         
             
                 
     
         
     
     
         
         
         
         
         
                 
     
     
     
             

     
         
             
             
                 
                 
                     
                 
                 
                                 
             
         
     
     
         
                     
             
         	
             
                 
             
         	
         	
                 
                     
         
     
     
     
         
         
		   
         	
         	
         	
     

     
         
         
    

 
`

上述代码请理解后根据实际情况进行修改，谢谢合作

**Tips:如果运行Build.xml提示：“Unable to locate tools.jar. Expected to find it in C:\Program Files\Java\jre6\lib”请将“C:\Program Files\Java\jdk1.6.0_16\lib”目录下的tools.jar文件拷贝到“C:\Program Files\Java\jre6\lib”目录下。**

**运行设置**

首先将整个Project都SVN Commit到之前架设好的SVN Server，这个就不具体说了

**配置Hudson**

1.新建一个“Build a free-style software project” 类型的job

2.在Source Code Management 里选择Subversion . 然后输入Repository URL 是你的Project Root 在SVN的URL

3.Build Triggers 里勾选Poll SCM， Schedule 里输入 * * * * * 星号之间有空格，表示 每一分钟检查一下，如果有更新就运行。Build periodically 勾选， Schedule 里输入 @midnight 表示 每天晚上运行一次，不管有没有更新

4.Build 里面选 Invoke Ant，然后Build File里面填写你的Build.xml的路径

5.Post-build Actions 可以更具自己需求设置

----------以上 配置可以按照自己需求随意更改，可以参看hudson里面的帮助信息-----------------------

**此文大部分内容参考了Bruce发表在SeleniumCN论坛的帖子，并根据自己搭建的经验做了多出修改，欢迎大家（[猛击这里](http://seleniumcn.cn/read.php?tid=164)）围观**

**后续更新：ReportNG+ Loggingselenium **
