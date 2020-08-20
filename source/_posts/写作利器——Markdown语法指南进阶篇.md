---
title: 写作利器——Markdown语法指南进阶篇
tags:
  - 写作指南
  - Markdown
categories:
  - 写作指南
  - Markdown
keywords: "Markdown 指导手册"
date: 2020-08-20 18:28:00
top_img: https://i.loli.net/2020/07/30/d4Y6PZqCrkusSIm.jpg
cover: https://i.loli.net/2020/07/30/7GPV1Q6wjIq5EyL.jpg
---

此篇文章是上一篇[《写作利器——Markdown语法指南基础篇》](https://daisy-li.github.io/2020/07/30/%E5%86%99%E4%BD%9C%E5%88%A9%E5%99%A8%E2%80%94%E2%80%94Markdown%E8%AF%AD%E6%B3%95%E6%8C%87%E5%8D%97%E5%9F%BA%E7%A1%80%E7%AF%87/)的进阶版，上一篇主要介绍 `Markdown` 的基础语法，如果你已经完全掌握上一篇的知识，现在你已经可以轻松的完成一篇文章的撰写了，那为什么会有后面的进阶版呢？其实就是介绍一些 `Markdown` 的小技巧和不常用到的图表类展示, 最后教大家如何在 `Markdown` 中使用 emoji，让你的文章更鲜活~ 😉

本文我将从以下四个方面总结，如后期有更新，可以关注我的个人博客和公众号。

1. 转义字符
2. 常用数学公式
3. 流程图
4. emoji

以下是正文

## 转义字符

> Markdown 利用很多特殊符号标识语法，但是需要输入这些符号就需要利用 `转义字符` 来控制，避免 Markdown 语法解析。

#### 1. Markdown中使用反斜杠 `\` 作为转义字符

例如我想单独输入 `*`，直接输入则直接被解析为无序列表，此时在星号前面加上转义字符就可以正常展示如下：
\*

Markdown 中常用的符号有：

```
\*
\*\*
\-
\>
\[\]
\(\)
```

#### 2. 使用 HTML 中的转义字符

html中包含一系列字符实体（特殊符号），利用转义字符可以将这些在html中 `具有特殊语法含义的字符作为普通字符输出`。

常用转义字符表：[HTML转义字符](https://tool.oschina.net/commons?type=2)

**PS: 记得转义符号后加分号~**

## 常用数学公式

插入公式的格式分为两种：

1. 行内公式：将公式插入到本行内，符号：$公式内容$，如：`$xyz$`
2. 独行公式：将公式插入到新的一行内，并且居中，符号：$$公式内容$$，如：`$$xyz$$`'

#### 上标、下标与组合

* 上标符号，符号：^，如：`$x^4$` 显示为 $x^4$
* 下标符号，符号：_，如：`$x_1$` 显示为 $x_1$
* 组合符号，符号：{}，如：`$x = a_1^n + a_{23}^n + a_{4}^n$` 显示为 $x = a_1^n + a_{23}^n + a_{4}^n$

#### 四则运算

1. 加法运算，符号：+，如：`$x+y=z$` $x+y=z$
2. 减法运算，符号：-，如：`$x-y=z$` $x-y=z$
3. 加减运算，符号：\pm，如：`$x \pm y=z$` $x \pm y=z$`
4. 减甲运算，符号：\mp，如：`$x \mp y=z$` $x \mp y=z$
5. 乘法运算，符号：\times，如：`$x \times y=z$` $x \times y=z$
6. 点乘运算，符号：\cdot，如：`$x \cdot y=z$` $x \cdot y=z$
7. 星乘运算，符号：\ast，如：`$x \ast y=z$` $x \ast y=z$
8. 除法运算，符号：\div，如：`$x \div y=z$` $x \div y=z$
9. 斜法运算，符号：/，如：`$x/y=z$` $x/y=z$
10. 分式表示，符号：\frac{分子}{分母}，如：`$\frac{x+y}{y+z}$` $\frac{x+y}{y+z}$
11. 分式表示，符号：{分子} \voer {分母}，如：`${x+y} \over {y+z}$` ${x+y} \over {y+z}$
12. 绝对值表示，符号：||，如：`$|x+y|$` $|x+y|$

#### 高级运算

1. 平均数运算，符号：\overline{算式}，如：`$\overline{xyz}$` $\overline{xyz}$
2. 开二次方运算，符号：\sqrt，如：`$\sqrt x$` $\sqrt x$
3. 开方运算，符号：\sqrt[开方数]{被开方数}，如：`$\sqrt[3]{x+y}$` $\sqrt[3]{x+y}$
4. 对数运算，符号：\log，如：`$\log(x)$` $\log(x)$
5. 极限运算，符号：\lim，如：`$\lim^{x \to \infty}_{y \to 0}{\frac{x}{y}}$` $\lim^{x \to \infty}_{y \to 0}{\frac{x}{y}}$
6. 求和运算，符号：\sum，如：`$\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}$` $\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}$
7. 积分运算，符号：\int，如：`$\int^{\infty}_{0}{xdx}$` $\int^{\infty}_{0}{xdx}$
8. 微分运算，符号：\partial，如：`$\frac{\partial x}{\partial y}$` $\frac{\partial x}{\partial y}$

#### 逻辑运算符

1. 大于等于运算，符号：\geq，如：`$x+y \geq z$` $x+y \geq z$
2. 小于等于运算，符号：\leq，如：`$x+y \leq z$` $x+y \leq z$
3. 不等于运算，符号：\neq，如：`$x+y \neq z$` $x+y \neq z$
4. 不大于等于运算，符号：\ngeq，如：`$x+y \ngeq z$` $x+y \ngeq z$
5. 不大于等于运算，符号：\not\geq，如：`$x+y \not\geq z$` $x+y \not\geq z$
6. 不小于等于运算，符号：\nleq，如：`$x+y \nleq z$` $x+y \nleq z$
7. 不小于等于运算，符号：\not\leq，如：`$x+y \not\leq z$` $x+y \not\leq z$
8. 约等于运算，符号：\approx，如：`$x+y \approx z$` $x+y \approx z$
9. 恒定等于运算，符号：\equiv，如：`$x+y \equiv z$` $x+y \equiv z$

## 流程图

> 写总结类文章经常会用到流程图来记录项目的时间线，或者用流程图来简明化的表示一些复杂的逻辑，以前我都是在 Visio 画好后直接以图片的形式引入，后来我发现其实 Markdown 也可以绘制流程图，让我们一起来看下如何操作吧。

流程图的绘制大致分为两段，第一段是定义元素，第二段是定义元素之间的走向。

#### 定义元素

```
tag=>type: content:>url 
```

* tag：就是一个标签，在第二段连接元素时用
* type：是这个标签的类型，一共有6种类型，分别是：
    1. start # 开始
    2. end # 结束
    3. operation #操作
    4. subroutine #子程序
    5. condition #条件
    6. inputoutput #输入输出
* content：流程语句中放置的内容
* url：链接，与流程语句绑定

**注意: type:与content之间一定要有一个空格，否则会出问题！**

#### 连接元素

用->来连接两个元素，需要注意的是condition类型，因为他有yes和no两个分支，所以要写成：

```
condition(yes)->op1->e 
condition(no)->op2->e
```

最后将两步合起来就构成了一个流程图，例如：

#### 代码

```
flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```

#### 效果

flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op

## 玩转 Emoji

#### 通用代码 Unicode

**使用格式为：`&#x`+`unicode`+`;`**

![emoji](https://i.loli.net/2020/08/20/NHyh6tG5cs9V312.jpg)

例如：

| Unicode | Markdown | emoji  |
| :---: | :---: | :---: |
| U+1F600 | `&#x1F600;` | &#x1F600; |
| U+1F349 | `&#x1F349;` | &#x1F349; |
| U+1F4AF | `&#x1F4AF;` | &#x1F4AF; |

Unicode列表参考：[Full Emoji List](http://www.unicode.org/emoji/charts/full-emoji-list.html#1f497)

#### Github Emoji

**使用格式为：`:`+`对应英文单词`+`:`**

![eemoji2](https://i.loli.net/2020/08/20/97kdam4vWofGFc3.jpg)

例如：
`:star:` :star:

这种格式只用部分 Markdown 平台支持，相比 Unicode 方便记忆，但是通用性不如 Unicode。

Emoji 简易版列表参考：[EMOJI CHEAT SHEET](https://www.webfx.com/tools/emoji-cheat-sheet/)

#### 搜狗输入法获取

最后给大家介绍一个最便捷获取表情的方法，没有任何格式限制，直接通过搜狗输入法获取，输入你想使用的表情中文，在搜狗输入法里面就会帮你匹配表情，如果没有，可以通过查看全部emoji，挑选合适的表情即可。

![搜狗](https://i.loli.net/2020/08/20/ydGKRFMvQ1Vetu8.jpg)

最后再教大家一个小技巧，可以使用标题格式改变表情的的大小，例如：

```
## 😀

### 😀

##### 😀
```

效果：
## 😀

### 😀

##### 😀

## 总结

总得的来说，MarkDown是一种简单、轻量级的标记语法，它是基于HTML之上，使用简洁的语法代替了排版，最终可以转换为PDF或HTML格式，方便我们快速做总结或书写文档。掌握了文章中的这些小技巧，不仅可以快速完成文章的撰写，还可以提升文章的点击率，让你的文章看起来更高大上~


