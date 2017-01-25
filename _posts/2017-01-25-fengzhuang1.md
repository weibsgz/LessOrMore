---
layout: post
title:  封装js
date:   2017-01-25 01:08:00 +0800
categories: js
tag: js
---

* content
{:toc}


HTML
====================================


{% highlight bash %}
<!DOCTYPE html>

<head>
    <title>css3滑动搜索</title>
    <meta charset="UTF-8">
    <style>
    html,
    body {
        height: 100%;
        overflow: hidden;
    }
    
    .main {
        width: 900px;
        margin: 0 auto;
        font-size: 0;
    }
    
    .main .tab {
        display: inline-block;
        width: 33%;
        height: 40px;
        line-height: 40px;
        text-align: center;
        border: 1px solid #ccc;
        font-size: 14px;
        border-right: 0px
    }
    
    .main .tab:last-child {
        border-right: 1px solid #ccc;
        position: relative;
    }
    
    .tab i {
        width: 100%;
        height: 2px;
        background-color: red;
        display: none;
        transition: all .5s
    }
    
    .active {
        color: red;
    }
    
    .active i {
        display: block;
    }
    
    .tab a {
        display: inline-block;
        transition: all 0.8s ease-in-out;
    }
    
    .tab a:hover {
        color: green;
        transform: rotateY(360deg);
    }
    
    #dialg {
        width: 400px;
        height: 400px;
        z-index: 10000;
        background-color: #fff;
        position: absolute;
        display: none;
    }
    
    #dialg .close {
        position: absolute;
        right: 0;
        top: 0
    }
    </style>
</head>

<body>
    <div class="main">
        <div class="tab active"><a>标题1</a><i></i></div>
        <div class="tab"><a>标题1</a><i></i></div>
        <div class="tab"><a>标题1</a><i></i></div>
    </div>
    <div class="content">1111
        <button id="tcBtn">弹出层</button>
    </div>
    <div class="content" style="display: none;">2222</div>
    <div class="content" style="display: none;">3333</div>
    <div id="dialg">
        <p>弹出层自己写</p>
        <div class="close">关闭</div>
    </div>
    <script src="jquery-1.10.1.min.js"></script>
    <script type="text/javascript">
    var Util = (function() {
        var tab = function(el, className, content) {
            var index = $(el).index();
            $(el).addClass(className).siblings().removeClass(className);
            $(content).eq(index).show().siblings(content).hide()
        }
        var tc = function(containerId) {
            
            if (document.getElementById('mengceng') === null) {
                var maskHtml = '<div id="mengceng"></div>';
                $("body").append(maskHtml);
                $("#mengceng").css({
                    "background-color": "#000",
                    "width": "100%",
                    "height": "100%",
                    "position": "absolute",
                    "z-index": "9999",
                    "left": "0",
                    "top": "0",
                    "opacity": "0.6"
                });
            }
            else{
              $("#mengceng").show()
            }



            $(containerId).show();
            $(containerId).css({
                    "position": "absolute",
                    "left": "0",
                    "top": "0",
                    "bottom": "0",
                    "right": "0",
                    "margin": "auto"
                })
                /*$("body").append(function(i,html){
                   return $(containerId).html();
                });*/
        }
        var closeTc = function(containerId) {
            $("#mengceng").hide();
            $(containerId).hide()
        }
        return {
            tab: tab,
            tc: tc,
            closeTc: closeTc
        }
    })();

    $(function() {
        $(".tab").on('click', function() {
            Util.tab(this, "active", ".content")
        })

        $("#tcBtn").on("click", function() {
            Util.tc("#dialg")
        })

        $(".close").on("click", function() {
            Util.closeTc("#dialg")
        })
    })
    </script>
</body>

</html>

{% endhighlight %}