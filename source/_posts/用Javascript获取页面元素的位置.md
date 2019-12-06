---
title: 用Javascript获取页面元素的位置
date: 2019-12-05 10:56:23
categories:
  - [JavaScript]
tags:
  - JS
---

制作网页的过程中，你有时候需要知道某个元素在网页上的确切位置。
下面的教程总结了 **Javascript** 在网页定位方面的相关知识。

<!--more-->

## 获取网页的大小

网页上的每个元素，都有 `clientHeight` 和 `clientWidth` 属性。这两个属性指元素的内容部分再加上 `padding` 的所占据的视觉面积，不包括 `border` 和滚动条占用的空间。

```js
function getViewport() {
  if (document.compatMode == "BackCompat") {
    return {
      width: document.body.clientWidth,
      height: document.body.clientHeight
    };
  } else {
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    };
  }
}
```

上面的 `getViewport` 函数就可以返回浏览器窗口的高和宽。使用的时候，有三个地方需要注意：

{% note warning %}
1）这个函数必须在页面加载完成后才能运行，否则 **document** 对象还没生成，浏览器会报错。

2）大多数情况下，都是 `document.documentElement.clientWidth` 返回正确值。但是，在 **IE6** 的**quirks** 模式中，`document.body.clientWidth` 返回正确的值，因此函数中加入了对文档模式的判断。

3）**clientWidth** 和 **clientHeight** 都是只读属性，不能对它们赋值。
{% endnote %}

## 获取网页大小的另一种方法

网页上的每个元素还有 `scrollHeight` 和 `scrollWidth` 属性，指包含滚动条在内的该元素的视觉面积。
那么，**document** 对象的 `scrollHeight` 和 `scrollWidth` 属性就是网页的大小，意思就是滚动条滚过的所有长度和宽度。仿照 **getViewport()** 函数，可以写出 **getPagearea()** 函数。

但是，这个函数有一个问题。如果网页内容能够在浏览器窗口中全部显示，不出现滚动条，那么网页的 `clientWidth` 和 `scrollWidth` 应该相等。但是实际上，不同浏览器有不同的处理，这两个值未必相等。所以，我们需要取它们之中较大的那个值，因此要对 **getPagearea()** 函数进行改写。

```js
function getPagearea() {
  if (document.compatMode == "BackCompat") {
    return {
      width: Math.max(document.body.scrollWidth, document.body.clientWidth),
      height: Math.max(document.body.scrollHeight, document.body.clientHeight)
    };
  } else {
    return {
      width: Math.max(
        document.documentElement.scrollWidth,
        document.documentElement.clientWidth
      ),
      height: Math.max(
        document.documentElement.scrollHeight,
        document.documentElement.clientHeight
      )
    };
  }
}
```

## 获取网页元素的绝对位置

网页元素的绝对位置，指该元素的左上角相对于整张网页左上角的坐标。这个绝对位置要通过计算才能得到。

首先，每个元素都有 `offsetTop` 和 `offsetLeft` 属性，表示该元素的左上角与父容器（`offsetParent` 对象）左上角的距离。所以，只需要将这两个值进行累加，就可以得到该元素的绝对坐标。

```js
function getElementLeft(element) {
  var actualLeft = element.offsetLeft;
  var current = element.offsetParent;

  while (current !== null) {
    actualLeft += current.offsetLeft;
    current = current.offsetParent;
  }

  return actualLeft;
}

function getElementTop(element) {
  var actualTop = element.offsetTop;
  var current = element.offsetParent;

  while (current !== null) {
    actualTop += current.offsetTop;
    current = current.offsetParent;
  }

  return actualTop;
}
```

{% note default %}
由于在表格和 `iframe` 中，`offsetParent` 对象未必等于父容器，所以上面的函数对于表格和 `iframe` 中的元素不适用。
{% endnote %}

## 获取网页元素的相对位置

网页元素的相对位置，指该元素左上角相对于浏览器窗口左上角的坐标。

有了绝对位置以后，获得相对位置就很容易了，只要将绝对坐标减去页面的滚动条滚动的距离就可以了。滚动条滚动的垂直距离，是 **document** 对象的 `scrollTop` 属性；滚动条滚动的水平距离是 **document** 对象的 `scrollLeft` 属性。

```js
function getElementViewLeft(element) {
  var actualLeft = element.offsetLeft;
  var current = element.offsetParent;

  while (current !== null) {
    actualLeft += current.offsetLeft;
    current = current.offsetParent;
  }

  if (document.compatMode == "BackCompat") {
    var elementScrollLeft = document.body.scrollLeft;
  } else {
    var elementScrollLeft = document.documentElement.scrollLeft;
  }

  return actualLeft - elementScrollLeft;
}

function getElementViewTop(element) {
  var actualTop = element.offsetTop;
  var current = element.offsetParent;

  while (current !== null) {
    actualTop += current.offsetTop;
    current = current.offsetParent;
  }

  if (document.compatMode == "BackCompat") {
    var elementScrollTop = document.body.scrollTop;
  } else {
    var elementScrollTop = document.documentElement.scrollTop;
  }

  return actualTop - elementScrollTop;
}
```

{% note default %}
`scrollTop` 和 `scrollLeft` 属性是可以赋值的，并且会立即自动滚动网页到相应位置，因此可以利用它们改变网页元素的相对位置。另外，`element.scrollIntoView()` 方法也有类似作用，可以使网页元素出现在浏览器窗口的左上角。
{% endnote %}

{% blockquote
阮一峰
http://www.ruanyifeng.com/blog/2009/09/find_element_s_position_using_javascript.html
用Javascript获取页面元素的位置
%}{% endblockquote %}
