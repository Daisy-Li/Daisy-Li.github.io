---
title: ES6学习笔记（一）
tags: 
  - 技术
  - 前端
  - 基础
categories:
  - 读书笔记
  - ES6
keywords: "基础，读书笔记"
date: 2021-06-30 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/es6top.jpeg
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/src=http___img-bss.csdn.net_201703031056227323.jpg&refer=http___img-bss.csdn.jpeg
---

1. let 命令
2. 块级作用域
3. const 命令
4. 顶层对象的属性
5. globalThis 对象

#### let命令

for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

1. 不存在变量提升，即声明之前不可使用。
2. 暂时性死去（TDZ，temporal dead zone）在代码块内，使用let命令声明变量之前，该变量都是不可用的。
3. 不允许重复声明。

总结：ES6 规定暂时性死区和let、const语句不出现变量提升。

#### 块级作用域
ES5只有全局作用域和函数作用域，let实际上为 JavaScript 新增了块级作用域。

*ES6:*
* 允许在块级作用域内声明函数。
* 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
* 同时，函数声明还会提升到所在的块级作用域的头部。

``` javascript
// 块级作用域内部，优先使用函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

ES6 的块级作用域必须有大括号，如果没有大括号，JavaScript 引擎就认为不存在块级作用域。

``` javascript
if (true) let x = 1;
ERROR: Uncaught SyntaxError: Lexical declaration cannot appear in a single-statement context
```

#### const 命令

const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，但对象本身是可变的，所以依然可以为其添加新属性。

ES6 声明变量有六种方法

#### 顶层对象的属性

顶层对象，在浏览器环境指的是window对象，在 Node 指的是global对象。ES5 之中，顶层对象的属性与全局变量是等价的。

ES6中 var命令和function命令声明的全局变量，依旧是顶层对象的属性；但let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。

``` javascript
var a = 1; 
// 如果在 Node 的 REPL 环境，可以写成 global.a 
// 或者采用通用方法，写成 this.a 
window.a // 1 

let b = 1; 
window.b // undefined
```

原因：避免某些变量的滥用，不利于模块化和内存回收，同时尽量保证编译时就可以报出变量未声明的错误（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的，此时只有运行才能获取到顶层对象）

#### globalThis 对象

ES2020 在语言标准的层面，引入globalThis作为顶层对象。也就是说，任何环境下，globalThis都是存在的，都可以从它拿到顶层对象，指向全局环境下的this。

