---
title: 12种不宜使用的Javascript语法
date: 2019-12-05 11:39:53
categories:
  - [JavaScript]
tags:
  - JS
---

在《Javascript 语言精粹》附录 B 中，**Douglas Crockford** 列出了 12 种应该避免使用的 **Javascript** 语法，我觉得非常值得推广。

<!--more-->

## ==

**Javascript** 有两组相等运算符，一组是 `==` 和 `!=`，另一组是 `===` 和 `!==`。前者只比较值的相等，后者除了值以外，还比较类型是否相同。

请尽量不要使用前一组，永远只使用 `===` 和 `!==`。因为==默认会进行类型转换，规则十分难记。

## with

`with` 的本意是减少键盘输入。

但是，在实际运行时，解释器会首先判断变量是否存在，如果不存在的话，再判断全局变量是否存在。这样就导致了低效率，而且可能会导致意外，因此最好不要使用 `with` 语句。

## eval

`eval` 用来直接执行一个字符串。这条语句也是不应该使用的，因为它有性能和安全性的问题，并且使得代码更难阅读。

## continue

这条命令的作用是返回到循环的头部，但是循环本来就会返回到头部。所以通过适当的构造，完全可以避免使用这条命令，使得效率得到改善。

## switch 贯穿

`switch` 结构中的 `case` 语句，默认是顺序执行，除非遇到 `break`，`return` 和 `throw`。

贯穿容易出错，而且难以发现。建议避免 `switch` 贯穿，凡是有 `case` 的地方，一律加上 `break`。

## 单行的块结构

`if`、`while`、`do` 和 `for`，都是块结构语句，但是也可以接受单行命令。

单行不利于阅读代码，而且将来添加语句时非常容易出错。建议不管是否只有一行命令，都一律加上大括号。

## ++和--

递增运算符 `++` 和递减运算符 `--`，直接来自 **C** 语言，表面上可以让代码变得很紧凑，但是实际上会让代码看上去更复杂和更晦涩。因此为了代码的整洁性和易读性，不用为好。

## 位运算符

**Javascript** 完全套用了 **Java** 的位运算符，包括按位与 `&`、按位或 `|`、按位异或 `^`、按位非 `~`、左移 `<<`、带符号的右移 `>>` 和用 **0** 补足的右移 `>>>`。

这套运算符针对的是整数，所以对 **Javascript** 完全无用，因为 **Javascript** 内部，所有数字都保存为双精度浮点数。

**Javascript** 不得不将运算数先转为整数，然后再进行运算，这样就降低了速度。

## function 语句

在 **Javascript** 中定义一个函数，有两种写法：

```js
function foo() {}
```

和

```js
var foo = function() {};
```

两种写法完全等价。但是在解析的时候，前一种写法会被解析器自动提升到代码的头部，因此违背了函数应该先定义后使用的要求，所以建议定义函数时，全部采用后一种写法。

## 基本数据类型的包装对象

**Javascript** 的基本数据类型包括字符串、数字、布尔值，它们都有对应的包装对象 `String`、`Number` 和 `Boolean`。所以，有人会这样定义相关值：

```js
new String("Hello World");

new Number(2000);

new Boolean(false);
```

这样写完全没有必要，而且非常费解，因此建议不要使用。

另外，`new Object` 和 `new Array` 也不建议使用，可以用 `{}` 和 `[]` 代替。

## new 语句

类是这样定义的：

```js
var Cat = function(name) {
  this.name = name;
  this.saying = "meow";
};
```

然后，再生成一个对象

```js
var myCat = new Cat("mimi");
```

这种利用函数生成类、利用 `new` 生成对象的语法，其实非常奇怪，一点都不符合直觉。而且，使用的时候，很容易忘记加上 `new`，就会变成执行函数，然后莫名其妙多出几个全局变量。所以，建议不要这样创建对象，而采用一种变通方法。

```js
Object.beget = function(o) {
  var F = function(o) {};
  F.prototype = o;
  return new F();
};
```

创建对象时就利用这个函数，对原型对象进行操作：

```js
var Cat = {
  name: "",
  saying: "meow"
};

var myCat = Object.beget(Cat);
```

对象生成后，可以自行对相关属性进行赋值：

```js
myCat.name = "mimi";
```

## void

在大多数语言中，`void` 都是一种类型，表示没有值。但是在 **Javascript** 中，`void` 是一个运算符，接受一个运算数，并返回 **undefined**。

```js
void 0; // undefined
```

这个命令没什么用，而且很令人困惑，建议避免使用。

{% blockquote
阮一峰
http://www.ruanyifeng.com/blog/2010/01/12_javascript_syntax_structures_you_should_not_use.html
12种不宜使用的Javascript语法
%}{% endblockquote %}
