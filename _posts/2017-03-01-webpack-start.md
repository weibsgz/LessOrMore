---
layout: post
title:  webpack-start
date:   2017-03-1 01:08:00 +0800
categories: 工具
tag: 工具
---

* content
{:toc}


安装步骤
====================================

1.npm init

2.进入目录  npm install webpack --save-dev

npm install webpack -g




注意事项
====================================
1.读取css需要loader
npm install css-loader style-loader --save-dev

在入口的JS文件引入
require('style-loader!css-loader!./style.css')


2.一些参数

webpack hello.js hello.bundle.js --watch --progress --display-modules --display-reasons


使用webpack.config.js配置
====================================
改写package.json   --config webpack.config.js webpack.config.js改名了也用这个 

{% highlight bash %}
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "webpack":"webpack --config webpack.config.js --watch --progress --display-modules --display-reason --colors"
  },
{% endhighlight %}

npm run webpack 运行



html-webpack-plugin
====================================
npm install html-webpack-plugin --save-dev


