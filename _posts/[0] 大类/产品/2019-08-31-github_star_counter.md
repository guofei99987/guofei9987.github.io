---
layout: post
title: 【小工具】github star数量统计
categories: 产品
tags:
keywords:
description:
---


一个统计某个账号下 github 的 star 数量的在线小应用。  

[应用地址](http://www.guofei.site/star_counter/main.html)  
输入账号，然后点击`Calculate` 按钮  

[源代码地址](https://github.com/guofei9987/star_counter)  



<!--
![demo](https://github.com/guofei9987/star_counter/blob/master/demo.png?raw=true) -->

## 试试吧：
<!-- <iframe src="http://www.guofei.site/star_counter/main.html" width="100%" height="1000em" marginwidth="10%"></iframe> -->



<script src="http://www.guofei.site/star_counter/star_counter.js"></script> <!--引用js代码-->
<script>
    function func_1() {
        document.getElementById("demo").innerHTML = 'If not print for seconds, please refresh';
        github_id = document.getElementById("user").value;
        document.getElementById("star_counter").innerHTML = func(github_id);
    }
</script>


Input github id: <input name="user" value="guofei9987" type="text" id="user">
<input name="Button" type="button" value="Calculate" onClick="func_1()">

<p id="star_counter"></p>
