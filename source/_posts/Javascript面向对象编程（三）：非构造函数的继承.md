---
title: Javascript 面向对象编程（三）：非构造函数的继承
date: 2019-12-05 16:43:11
categories:
  - [JavaScript]
tags:
  - Js
---

这个系列的 {% post_link Javascript面向对象编程（一）：封装 '第一部分' %} 介绍了“封装”，{% post_link Javascript面向对象编程（一）：封装 '第二部分' %} 介绍了使用构造函数实现“继承”。

今天是最后一个部分，介绍不使用构造函数实现“继承”。

<!--more-->

## 什么是“非构造函数”的继承？

比如，现在有一个对象，叫做"中国人"。

```js
var Chinese = {
  nation: "中国"
};
```

还有一个对象，叫做"医生"。

```js
var Doctor = {
  career: "医生"
};
```

请问怎样才能让“医生”去继承“中国人”，也就是说，我怎样才能生成一个“中国医生”的对象？

这里要注意，这两个对象都是普通对象，不是构造函数，无法使用构造函数方法实现“继承”。

## object()方法

**json** 格式的发明人 **Douglas Crockford**，提出了一个 **object()** 函数，可以做到这一点。

```js
function object(o) {
  function F() {}

  F.prototype = o;

  return new F();
}

var Doctor = object(Chinese); // 第一步先在父对象的基础上，生成子对象

Doctor.career = "医生"; // 再加上子对象本身的属性

alert(Doctor.nation); //中国
```

这个 **object()** 函数其实只做一件事，就是把子对象的 `prototype` 属性，指向父对象，从而使得子对象与父对象连在一起。

## 浅拷贝

除了使用“prototype 链”以外，还有另一种思路：把父对象的属性，全部拷贝给子对象，也能实现继承。

```js
function extendCopy(p) {
  var c = {};

  for (var i in p) {
    c[i] = p[i];
  }

  c.uber = p;

  return c;
}

var Doctor = extendCopy(Chinese);

Doctor.career = "医生";

alert(Doctor.nation); // 中国
```

这样的拷贝有一个问题。如果父对象的属性等于数组或另一个对象，那么实际上，子对象获得的只是一个内存地址，而不是真正拷贝，因此存在父对象被篡改的可能。

所以，**extendCopy()** 只是拷贝基本类型的数据，我们把这种拷贝叫做“浅拷贝”。

## 深拷贝

所谓“深拷贝”，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难，只要递归调用“浅拷贝”就行了。

```js
function deepCopy(p, c) {
  var c = c || {};

  for (var i in p) {
    if (typeof p[i] === "object") {
      c[i] = p[i].constructor === Array ? [] : {};

      deepCopy(p[i], c[i]);
    } else {
      c[i] = p[i];
    }
  }

  return c;
}

var Doctor = deepCopy(Chinese);
```

{% blockquote
阮一峰
http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html
Javascript 面向对象编程（三）：非构造函数的继承
%}{% endblockquote %}
