---
title: 学习Javascript闭包（Closure）
date: 2019-12-04 17:57:49
categories:
  - [JavaScript]
tags:
  - Js
---

闭包（`closure`）是 **Javascript** 语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现。

<!--more-->

## 闭包的概念

闭包（`closure`）就是能够读取其他函数内部变量的函数。

由于在 **Javascript** 语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。

所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

## 闭包的用途

一个是可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

## 闭包的注意点

{% note warning %}
1). 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在 **IE** 中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

2). 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（**object**）使用，把闭包当作它的公用方法（**Public Method**），把内部变量当作它的私有属性（**private value**），这时一定要小心，不要随便改变父函数内部变量的值。
{% endnote %}

{% blockquote
阮一峰
http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
学习Javascript闭包（Closure）
%}{% endblockquote %}
