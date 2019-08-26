---
layout: post
title: 【JavaScript】入门
categories: 前端
tags:
keywords:
description:
---

## 函数

```JavaScript
function plus1(x) {
    return x + 1;
}
```

### 方法的使用

```JavaScript
var a = [];
a.push(1, 2, 3); //添加新元素
a.reverse();//反转，a变化了
```

### 自定义方法

定义

```JavaScript
points.dist = function () {
    var p1 = this[0];
    var p2 = this[1];
    var a = p2.x - p1.x;
    var b = p2.y - p1.y;
    return Math.sqrt(a * a + b * b);
};

//使用
points.dist();
```

## 控制语句

if

```JavaScript
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

while

```JavaScript
function factorial(n) {
    var product = 1;
    while (n > 1) {
        product *= n;
        n--;
    }
    return product
}
```

for

```JavaScript
function factorial2(n) {
    var i, product = 1;
    for (i = 2; i <= n; i++) {
        product *= i;
    }
    return product;
}
```

## 面向对象

```JavaScript
function Point(x, y) {//惯例：构造函数均以大写字母开头
    this.x = x;
    this.y = y;
//不需要return
}

//生成对象
var p = new Point(1, 1);

//定义方法
Point.prototype.r = function () {
    return Math.sqrt(this.x * this.x + this.y * this.y);
};

//使用方法
p.r()
```
