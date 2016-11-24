---
layout: post
title:  vue cli安装
date:   2016-11-15 01:08:00 +0800
categories: vue
tag: vue
---

* content
{:toc}


前言
====================================
利用vue-cli 快速构建VUE项目

+ 感谢[Less官网](http://lesscss.cn/)的样式，本Jekyll框架的样式都是基于Less官网的样式直接拷贝过来的。只是重构了JS，并且加入了Jekyll语法而已。
+ 感谢[Github](https://github.com/)提供的代码维护和发布平台
+ 感谢[Jekyll](https://jekyllrb.com/)团队做出如此优秀的产品
+ 感谢[Solar](https://github.com/mattvh/solar-theme-jekyll)的原作者[Matt Harzewski](http://www.webmaster-source.com/)，在`2014.11`-`2016.09`的两年间，我的博客选用了此样式模版


安装步骤
====================================
{% highlight bash %}
$ npm install -g vue-cli

$ vue init webpack 《你的项目名称》
//安装1.0版本
vue init webpack#1.0 sell
{% endhighlight %}

配置一些名字 描述什么的 就可以了

然后 cd 你的项目 进入项目目录
npm install
npm run dev  运行完毕后会启动服务 默认监听localhost:8080


关于目录结构中的.eslintrc.js
====================================
这个是检查ES6语法的，双击打开
可以看到 [github上的规则地址](https://github.com/feross/standard/blob/master/RULES.md#javascript-standard-style)
并且可以修改，找到规则名字 变成0就可以了。
{% highlight bash %}
'rules': {
    // allow paren-less arrow functions
    'arrow-parens': 0,
    // allow async-await
    'generator-star-spacing': 0,
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0
  }
 {% endhighlight %}