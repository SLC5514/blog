---
title: Javascript 的 this 用法
date: 2019-12-05 13:24:24
categories:
  - [JavaScript]
tags:
  - Js
---

`this` 是 **JavaScript** 语言的一个关键字。

它是函数运行时，在函数体内部自动生成的一个对象，只能在函数体内部使用。

函数的不同使用场合，`this` 有不同的值。总的来说，`this` 就是函数运行时所在的环境对象。

<!--more-->

## 情况一：纯粹的函数调用

这是函数的最通常用法，属于全局性调用，因此 `this` 就代表全局对象。

```js
function test() {
  this.x = 1;
}
```

## 情况二：作为对象方法的调用

函数还可以作为某个对象的方法调用，这时 `this` 就指这个上级对象。

```js
function test() {
  console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;

obj.m(); // 1
```

## 情况三 作为构造函数调用

所谓构造函数，就是通过这个函数，可以生成一个新对象。这时，`this` 就指这个新对象。

```js
function test() {
  this.x = 1;
}

var obj = new test();
obj.x; // 1
```

## 情况四 apply 调用

`apply()` 是函数的一个方法，作用是改变函数的调用对象。它的第一个参数就表示改变后的调用这个函数的对象。因此，这时 `this` 指的就是这第一个参数。

```js
var x = 0;
function test() {
  console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;
obj.m.apply(); // 0
```

{% blockquote
阮一峰
http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html
Javascript 的 this 用法
%}{% endblockquote %}
