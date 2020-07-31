---
title: 写作利器——Markdown语法指南基础篇
tags:
  - 写作指南
  - Markdown
categories:
  - 写作指南
  - Markdown
keywords: "Markdown 指导手册"
date: 2020-07-30 18:33:00
top_img: https://i.loli.net/2020/07/30/d4Y6PZqCrkusSIm.jpg
cover: https://i.loli.net/2020/07/30/7GPV1Q6wjIq5EyL.jpg
---

## 前言

最近因为工作的原因，需要帮助组内制定文章书写规范，审查文章格式，目前文章均按照 Markdown 格式书写，我发现虽然大家都能正常的完成一篇文章，但是在格式细节上还有很多问题，为了帮助大家规范文章格式，特此整理一下 Markdown 基本规范，方便后面的小伙伴更好的畅所欲言~

##  为什么选择 Markdown？

在学习语法之前，我们先来看下什么是 [Markdown](https://zh.wikipedia.org/wiki/Markdown)：

> Markdown是一种轻量级标记语言，创始人为约翰·格鲁伯（英语：John Gruber）。 它允许人们使用易读易写的纯文本格式编写文档，文档后缀为 `.md`。
> 目前许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。 如GitHub、Reddit、Diaspora、Stack Exchange、OpenStreetMap 、SourceForge、简书等，甚至还能被使用来撰写电子书。

鉴于 Markdown 以上的特性，用 Markdown 有如下好处：

1. 什么人都能打开，因为是纯文本，所以不需要特殊的软件，最简单的记事本就可以浏览；
2. 转HTML非常方便，HTML是 web 的标记语言，用 Markdown 标注格式之后，可以很方便的转为以上格式；
3. Markdown让我更专注写作，而不是排版

看了以上这么多优点，你是不是很心动呢，接下来就让我们来学习 Markdown 的语法吧，虽然 Markdown 的语法已经足够简单，但是还是有很多细节需要我们注意，每一种格式之后我会根据自己的写作经验提出一些自己的见解，帮助大家更合理的运用每一种格式。 

## 基本语法

### 全局规范

- MarkDown 文件均使用 `.md` 作为后缀 (小写字母)
- 普通文本换行，使用行末尾`2空格`触发

### 标题

在 Markdown 中，如果一段文字被定义为标题，只要在这段文字前加 `#` 即可：

`# 一级标题`

`## 二级标题`

`### 三级标题`

以此类推，总共有六级标题，需要注意的是 `#` 号和文字中间要有一个空格，标题与上下文之间使用空行隔开。  

PS：建议行文从二级标题开始使用哦~

### 强调

- `**加粗**` 相当于  `<strong></strong>` 标签，表现为加粗，例如：此处需要**加强**表示；
- `*斜体*` 相当于 `<em></em>` 标签，表现为倾斜，例如：此处需要*特殊*表示；
- `~~删除线~~` 表现为给文字添加删除线，例如：此处需要 ~~删除线~~

### 列表

* 有序列表的显示只需要在文字前使用`1. 2. `符号即可；
* 无序列表使用`-`或者`*`标识；
* 列表标识符号和其后面的内容使用`空格`分开；
* 列表内嵌套列表时，在第二层列表前面加`tab`缩进即可；
* 列表块前后`整行隔开`。

```
1. 7:00~9:00 自我投资 大脑的黄金时间
2. 9:00~12:00 专注时间 罐头工作术
3. 12:00~13:00 吃午餐 血清素提升
    * 谷物
    * 水果
    * 蔬菜
4. 13:00~16:00 非专注性工作
```

实际预览效果：

1. 7:00~9:00 自我投资 大脑的黄金时间
2. 9:00~12:00 专注时间 罐头工作术
3. 12:00~13:00 吃午餐 血清素提升
    * 谷物
    * 水果
    * 蔬菜
4. 13:00~16:00 非专注性工作

### 代码块规范

* 行内代码使用`1对波浪号` 如： `hello world!`；
* 块级代码使用`3个波浪号` 或 `整体4空格缩进`，且上下均用整行隔开；
* 代码块支持部分语法高亮，使用方式是在波浪号后加语法关键字，如下：

![代码块](https://i.loli.net/2020/07/30/Q7gPTLvZKJUoNnm.png)

#### 支持高亮显示的语言

我使用表格方式整理了 Markdown 目前支持的高亮语法，方便后期查阅。

| 名称  | 关键字  | 调用的js  |
| :-- | :-- | :-- |
| AppleScript |	applescript | shBrushAppleScript.js	|
| ActionScript 3.0 | actionscript3 , as3 | shBrushAS3.js |	
| Shell | bash , shell |	shBrushBash.js |
| ColdFusion | coldfusion , cf | shBrushColdFusion.js |	
| C | cpp , c | shBrushCpp.js |	
| C# | c# , c-sharp , csharp | shBrushCSharp.js |
| CSS | css | shBrushCss.js |
| Delphi | delphi , pascal , pas | shBrushDelphi.js |
| diff&patch | diff patch | shBrushDiff.js |
| Erlang | erl , erlang | shBrushErlang.js |
| Groovy | groovy | shBrushGroovy.js |
| Java | java | shBrushJava.js |
| JavaFX | jfx , javafx | shBrushJavaFX.js |	
| JavaScript | js , jscript , javascript | shBrushJScript.js |
| Perl | perl , pl , Perl | shBrushPerl.js |
| PHP | php | shBrushPhp.js |
| text | text , plain | shBrushPlain.js |
| Python | py , python | shBrushPython.js |	
| Ruby | ruby , rails , ror , rb | shBrushRuby.js |
| SASS&SCSS | sass , scss | shBrushSass.js |
| Scala | scala | shBrushScala.js |
| SQL | sql | shBrushSql.js |
| Visual Basic | vb , vbnet | shBrushVb.js |
| XML | xml , xhtml , xslt , html | shBrushXml.js |
| Objective C | objc , obj-c | shBrushObjectiveC.js |
| F# | f# f-sharp , fsharp | shBrushFSharp.js |
| R | r , s , splus | shBrushR.js |
| matlab | matlab | shBrushMatlab.js |
| swift | swift | shBrushSwift.js |
| GO | go , golang | shBrushGo.js |

### 引用

引用需要在被引用的文正前加`>`符号，引用内容每行前面都需要添加`>`符号，一旦断开就会默认为下段引用。

```
>这是一句引用
>> 这是嵌套引用
```

显示效果：
>这是一句引用
>> 这是嵌套引用

### 链接

#### 行内形式

`[链接文字](链接地址 "title")`

#### 参考形式

```
[链接文字][id]

// 在文章末尾处添加
[id]: link "Title"
```

#### 自动链接

使用 <> 包括的 URL 或邮箱地址会被自动转换为超链接：

```
<http://www.baidu.com/>

<123@email.com>
```

<http://www.baidu.com/>

<123@email.com>

### 图片

#### 行内式

```
![image](image.link)

![demo](https://i.loli.net/2020/07/30/5r6ak4cvZhHUXlC.jpg)
```
![image](https://i.loli.net/2020/07/30/5r6ak4cvZhHUXlC.jpg "Markdown图片示例")

#### 参考式

在文档要插入图片的地方写`![图片Alt][标记]`

在文档的末尾写上`[标记]:图片地址 "Title"`，和链接同理。

```
![imgDemo][1]

[1]:https://i.loli.net/2020/07/30/5r6ak4cvZhHUXlC.jpg "Markdown图片示例"
```

![imgDemo][1]

[1]:https://i.loli.net/2020/07/30/5r6ak4cvZhHUXlC.jpg "Markdown图片示例"


### 表格

<preview>

| 表头 | 表头 | 表头 |

| :--- | ---: | :---: |

| 文字 | 文字 | 文字 |

| 文字| 文字 | 文字 |

</preview>

注： ` :---`代表左对齐，`---:`代表右对齐，`:---:`代表居中对齐，`-`数目至少一个，没有冒号默认左对齐，第二行必须有，否则不是表格形式。

**显示效果**：

| 左对齐 | 居中 | 右对齐 |
| :--- | :---: | ---: |
| CSS | css | shBrushCss.js |
| JavaScript | js , jscript , javascript | shBrushJScript.js |
| Shell | bash , shell |	shBrushBash.js |

### 分割线

你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：

```
* * *

***

*****

- - -

---------------------------------------
```

**显示效果**：

---

## 参考
[Markdown 官方文档](https://daringfireball.net/projects/markdown/syntax)