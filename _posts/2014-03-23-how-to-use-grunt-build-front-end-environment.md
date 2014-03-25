---
layout: post
title: "如何使用Grunt构建前端自动化开发环境"
description: 搭建自动化开发环境，作为前端同学的职业软技能，在一定程度上可以加不少分呢：）
categories:
- 蛋疼的事
tags:
- Grunt
- 前端软技能
- 大叔学技术


---

上礼拜有很大一部分工作量是面试实习生和社招，有很多前端同学的简历写的都不错，其中就有一项职业技能是「使用[Grunt](http://gruntjs.com/)/[Gulp](http://gulpjs.com/)/[FIS](http://fis.baidu.com/)构建前端自动化开发环境」，如果是潜心研究过确实可以作为前端软技能加不少分呢：）

去年底做海外淘宝搜索重构的项目时，断断续续的使用了Grunt在本地构建了一个环境，但是也没有仔细研究过，这个周末折腾业余项目时正好趁机重新熟悉了下Grunt

**踩坑提要：**

* npm经常抽风怎么办？
* LiveReolad与Connect是好基友
* 图片压缩imagemin 0KB解决办法
* 如何监测单一文件改动时，不会编译整个文件夹

**安装指南**

Grunt基于Node.js，所以需要先安装Node环境，直接杀至[官网](http://nodejs.org/)，Mac用户可以直接点击install下载pkg文件安装，2B青年可以选择自行编译...

安装好Node环境则使用一阳指，即可安装好grunt客户端环境（-g指的是安装到Global环境）

`sudo npm install grunt-cli -g`

但是npm最近经常抽风，经常遇到安装过程假死...肿么办，没关系，让大湿兄出马，关门...上[npm国内镜像](http://cnodejs.org/topic/4f9904f9407edba21468f31e)

`npm config set registry http://registry.cnpmjs.org `

在项目文件夹新建两个文件：package.json + Gruntfile.js

package.json 指定当前项目所需的模块

    {
      "name": "Grunt",
      "file": "Grunt",
      "author": "besteric",
      "version": "0.0.1",
      "description": "front-end auto-package environment",
      "license": "BSD",
      "devDependencies": {
      "grunt": "~0.4.1",
      "grunt-contrib-jshint": "~0.6.3",
      "grunt-contrib-uglify": "~0.2.1",
      "grunt-contrib-requirejs": "~0.4.1",
      "grunt-contrib-copy": "~0.5.0",
      "grunt-contrib-clean": "~0.5.0",
      "grunt-strip": "~0.2.1",
      "grunt-contrib-concat": "~0.3.0",
      "grunt-contrib-less": "~0.11.0",
      "grunt-contrib-watch": "~0.6.1",
      "grunt-regarde": "~0.1.1",
      "grunt-contrib-connect": "~0.7.1",
      "grunt-contrib-livereload": "~0.1.2",
      "connect-livereload": "~0.3.2",
      "grunt-newer": "~0.7.0",
      "grunt-contrib-imagemin": "~0.5.0",
      "grunt-contrib-cssmin": "~0.9.0",
      "grunt-htmlhint": "~0.4.0"
      },
      "dependencies": {
        "express": "3.x"
      }
    }

然后执行 `npm install` 即可在当前文件夹下自动下载Grunt以及需要用到的所有插件包（Grunt在0.4版本以后针对每一个项目都会加载单独的Grunt，避免不同项目的Grunt版本不一致导致的冲突）

安装完后可以看到目录多了一个文件夹：node_modules，如果使用Git管理项目，建议将此文件夹纳入.gitignore，以免push至服务器（琐碎文件过多）

最后再根据实际情况编写Grunt的配置文件——Gruntfile.js

    module.exports = function(grunt) {

      // 配置Grunt各种模块的参数
      grunt.initConfig({
      less: { /* less的参数 */ },
      watch:  { /* watch的参数 */ }
    });

      // 从node_modules目录加载模块文件
      grunt.loadNpmTasks('grunt-contrib-less');
      grunt.loadNpmTasks('grunt-contrib-watch');

      // 每行registerTask定义一个任务
      grunt.registerTask('default', ['less']);
      grunt.registerTask('build', ['less']);

    };



**部分插件实例**

* grunt-contrib-uglify

        uglify: {
          build: {
            files: [{
            expand: true,
            cwd: '0.2/assets/js',
            src: '*.js',
            dest: 'build/assets/js',
            ext:'.js'
          }],
            options: {
              banner: '/*! Creat by <%= pkg.author %> @<%= grunt.template.today("yyyy-mm-dd") %> */\n'
            }
          }
        }

* grunt-contrib-jshint

        jshint: {
          // define the files to lint
          files: ['gruntfile.js', '0.2/**/*.js'],
          // configure JSHint (documented at http://www.jshint.com/docs/)
          options: {
            // more options here if you want to override JSHint defaults
            globals: {
              //大括号包裹
              curly: true,
              //对于简单类型，使用===和!==，而不是==和!=
              eqeqeq: true,
              //对于属性使用aaa.bbb而不是aaa['bbb']
              sub: true,
              //查找所有未定义变量
              undef: true,
              //查找类似与if(a = 0)这样的代码
              boss: true,
              jQuery: true,
              console: true,
              module: true
            }
          }
        }

* grunt-contrib-connect（**无视浏览器，不用安装Liveroad浏览器插件**）
        
        // LiveReload的默认端口号，你也可以改成你想要的端口号
        var lrPort = 35729;
        // 使用connect-livereload模块，生成一个与LiveReload脚本
        // <script src="http://127.0.0.1:35729/livereload.js?snipver=1" type="text/javascript"></script>
        var lrSnippet = require('connect-livereload')({ port: lrPort });
        // 使用 middleware(中间件)，就必须关闭 LiveReload 的浏览器插件
        var lrMiddleware = function(connect, options) {
          return [
            // 把脚本，注入到静态文件中
            lrSnippet,
            // 静态文件服务器的路径
            connect.static(String(options.base)),
            // 启用目录浏览(相当于IIS中的目录浏览)
            connect.directory(String(options.base))
          ];
        };
        
        connect: {
          options: {
            // 服务器端口号
            port: 8080,
            hostname: 'localhost',
            base: '.'
          },
          livereload: {
            options: {
              // 通过LiveReload脚本，让页面重新加载。
              middleware: lrMiddleware
            }
          }
        }
        
* grunt-contrib-imagemin（**使用cache:false解决压缩后的图片为0KB的bug**）

        imagemin: {
          dynamic: {
            files: [{
              expand: true,
              cwd: '0.2/assets/img',
              src: ['**/*.{png,jpg,gif}'],
              dest: 'build/assets/img/'               
            }],
            options: {
              //解决图片压缩0KB的Bug
              cache: false
            }
          }
        }

* grunt-contrib-watch（实时监测文件变化，执行相应操作：刷新页面/语法检查/CSS预编译）
* grunt-newer（针对单一文件修改执行相应操作，而不是刷新整个目录）

        watch: {
          html: {
            files: ['0.2/pages/*.html'],
            tasks: ['newer:htmlhint']
          },
          less: {
            files: ['0.2/assets/style/less/*.less'],
            // newer只监测新增文件
            tasks: ['newer:less']
          },
          javascript: {
            files: ['*.js'],
            tasks: ['newer:jshint']
          },
          livereload: {
            // 我们不需要配置额外的任务，watch任务已经内建LiveReload浏览器刷新的代码片段。
            options: {
              livereload: lrPort
            },
            // '**' 表示包含所有的子目录
            // '*' 表示包含所有的文件
            files: ['0.2/pages/*','0.2/assets/**/*','!0.2/assets/style/css/*.css'],
         }
        }
        

其他插件可以参考下我写的[Gruntfile.js](https://gist.github.com/9725411)，在此不再复述

**参考信息：**

* [Grunt：任务自动管理工具](http://javascript.ruanyifeng.com/tool/grunt.html#toc7)
* [Getting started—grunt入门指南](http://www.36ria.com/6192)
* [Grunt插件之LiveReload 实现页面自动刷新，所见即所得编辑](http://www.bluesdream.com/blog/grunt-plugin-livereload-wysiwyg-editor.html)
* [Grunt: Watch multiple files, Compile only Changed](http://stackoverflow.com/questions/16788731/grunt-watch-multiple-files-compile-only-changed)
* [Build Wars (Grunt Vs Gulp)](http://markdalgleish.github.io/presentation-build-wars-gulp-vs-grunt/?utm_source=feweekly&utm_campaign=issue28&utm_medium=web)