---
title: React学习笔记——JSX编译
tags: 
  - react
categories:
  - 学习笔记
  - React
keywords: "react JSX"
date: 2021-12-08 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
---

### jsx

JSX 元素节点会被编译成 React Element 形式，使用React.creatElement方法。

`createElement` 参数：


-   第一个参数：如果是组件类型，会传入组件对应的类或函数；如果是 dom 元素类型，传入 div 或者 span 之类的字符串。
-   第二个参数：一个对象，在 dom 类型中为标签属性，在组件类型中为 props 。
-   其他参数：依次为 children，根据顺序排列。

举个例子：

```
<div>
   <TextComponent />
   <div>hello,world</div>
   let us learn React!
</div>
```

上面的代码会被 babel 先编译成：

```
 React.createElement("div", null,
        React.createElement(TextComponent, null),
        React.createElement("div", null, "hello,world"),
        "let us learn React!"
    )
```

### fiber

最终，在调和阶段，使用react reconciler（调和器），上述 React element 对象的每一个子节点都会形成一个与之对应的 fiber 对象，然后通过 sibling、return、child 将每一个 fiber 对象联系起来。

fiber tag 标识不用种类的fiber对象。

fiber 对应关系

-   child： 一个由父级 fiber 指向子级 fiber 的指针。
-   return：一个子级 fiber 指向父级 fiber 的指针。
-   sibiling: 一个 fiber 指向下一个兄弟 fiber 的指针。 

### 总结

JSX --- babel编译 ---> ReactElement --- reconciler ---> fiber
