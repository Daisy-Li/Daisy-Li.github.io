---
title: JS对Iframe内外页面进行操作
tags: 
  - 技术
  - Javascript
categories:
  - 前端
  - 经验沉淀
keywords: "iframe"
date: 2021-10-11 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/src=http___inews.gtimg.com_newsapp_match_0_10890976489_0.jpg&refer=http___inews.gtimg.jpeg
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/src=http___inews.gtimg.com_newsapp_match_0_10890976489_0.jpg&refer=http___inews.gtimg.jpeg
---

### 方式一
通过contentWindow和contentDocument这两个API:

```js
var iframe = document.getElementById("iframe1");
var iwindow = iframe.contentWindow;
var idoc = iwindow.document;
var idocument = iframe.contentDocument;
console.log("window",iwindow);//获取iframe的window对象
console.log("document",idoc);  //获取iframe的document
console.log("html",idoc.documentElement);//获取iframe的html
```
其中iframe.contentWindow可以获取iframe的window对象,iframe.contentDocument可以获取iframe的document对象。

### 方式二

结合Name属性，通过window提供的frames获取:

```html
<iframe src ="/index.html" id="ifr1" name="ifr2" scrolling="yes">
  <p>Your browser does not support iframes.</p>
</iframe>
<script type="text/javascript">
    console.log(window.frames['ifr2'].window);
    console.dir(document.getElementById("iframe").contentWindow);
</script>
```

## 在iframe内获取iframe外的内容


```js
window.parent 获取上一级的window对象，如果还是iframe则是该iframe的window对象
window.top 获取最顶级容器的window对象，即，就是你打开页面的文档
```

![37177448.jpg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbd94862dba4460d970290261cdf3bf2~tplv-k3u1fbpfcp-watermark.image?)

## 在iframe中调用父页面中定义的方法和变量

```js
window.parent.window.parentMethod();
window.parent.window.parentValue;
```

## 在父页面操作iframe子页面的方法和变量

```js
window.frames["iframe_Name"].window.childMethod();
window.frames["iframe_Name"].window.childValue;
```

## 总结
在使用Iframe时还需要注意以下两点：
1. 要确保在iframe加载完成后再进行操作，如果iframe还未加载完成就开始调用里面的方法或变量，会产生错误；
2. 如果iframe所链接的是外部页面，因为安全机制不能使用同域名下的通信方式；

### 判断iframe加载完成

```js
iframe.onload = function() {
    // TODO
}
```

### 不同域通信

**父页面向子页面传递数据**

利用location对象的hash值，通过它传递通信数据。在父页面设置iframe的src后面多加个data字符串，然后在子页面中通过某种方式能即时的获取到这儿的data。

**子页面向父页面传递数据**

利用一个代理iframe，它嵌入到子页面中，并且和父页面必须保持是同域，然后通过它充分利用上面同域通信方式的实现原理，把子页面的数据传递给代理iframe，然后由于代理的iframe和主页面是同域的，所以主页面就可以利用同域的方式获取到这些数据。
