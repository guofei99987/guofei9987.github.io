---
layout: post
title: 【小工具】震惊体生成器
categories: 产品
tags:
keywords:
description:
---

## demo
**因为博客架构的问题，如果点击按钮没反应，刷新一次就好了**


<script src="http://www.guofei.site/vulgar_language_generator/vulgar_poet/vulgar_poet.js"></script>
<script>
    // 这一段代码可以放到head/引用/body中，都可以调用
    function myFunction() {
        document.getElementById("vulgar_poet").innerHTML += vulgar_poet();
    }
</script>

<button type="button" onclick="myFunction()">恶俗古风诗歌生成器</button>
<p id="vulgar_poet"></p>

## 如何部署到你的网站上

```html
<script src="http://www.guofei.site/vulgar_language_generator/vulgar_poet/vulgar_poet.js"></script>
<script>
    // 这一段代码可以放到head/引用/body中，都可以调用
    function myFunction() {
        document.getElementById("vulgar_poet").innerHTML += vulgar_poet();
    }
</script>

<button type="button" onclick="myFunction()">恶俗古风诗歌生成器</button>
<p id="vulgar_poet"></p>
```
