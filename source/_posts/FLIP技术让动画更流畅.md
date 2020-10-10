---
title: FLIP技术让动画更流畅
tags: 
  - 技术
  - 前端
  - 性能
  - 动画
categories:
  - 前端
  - 经验沉淀
keywords: "性能优化，FLIP，动画"
date: 2020-09-29 15:00:00
top_img: https://i.loli.net/2020/10/10/lTWDpcSBInw6sML.jpg
cover: https://i.loli.net/2020/10/10/5ZVQCz8bSNDrYxI.png
---

## 前言

> 此篇文章实际上是《H5页面性能优化之加载篇》的姊妹篇。主要想从CSS 动画的角度给大家介绍下 FLIP 技术，可以帮助大家创建真正轻量级的动画，以此来提高 Web 动画的性能。

很多同学平时并不重视动画，认为动画相比较业务逻辑画蛇添足，有时还影响页面性能，所以开发时则是能省就省，其实不然，开发中业务逻辑当然是要放在首位的，但是同样也不要小看了动画，在某些特定场景下，其所能起到的作用甚至可以与业务的目标并驾齐驱。但是这有一个大前提，那就是需要流畅的动画，FLIP 技术应运而生，可以帮助你创建真正轻量级的动画。

## FLIP 技术

FLIP是一种记忆设备和技术，最早是由`@Paul Lewis`提出的，FLIP是First、Last、Invert和Play四个单词首字母的缩写。

FLIP 将一些性能低下的动画映射为 transform 动画。通过记录元素的两个快照，一个是元素的初始位置（First – F），另一个是元素的最终位置（Last – L），然后对元素使用一个 transform 变换来反转（Invert – I），让元素看起来还在初始位置，最后移除元素上的 transform 使元素由初始位置运动（Play – P）到最终位置。

它就是通过这样一种高性能的方式来动态的改变DOM元素的位置和尺寸，而不需要管它的布局是如何计算或渲染的（比如，height、width、float、绝对定位、Flexbox和Grid等）。

上面这个过程可以拆解为以下四个步骤：

#### 1. First

元素的初始状态，记录当前元素的位置、尺寸、透明度等等的样式信息

#### 2. Last

元素的最终状态，即动画后元素的位置、尺寸、透明度等等的样式信息

#### 3. Invert

将元素恢复至动画前状态，即相反操作，先计算出从初始状态到最终状态元素发生的改变，比如宽度、高度、透明度等，然后在元素上应用 transform 或 opacity 使这些改变反转，给人一种错觉，即它原来就在初始位置。

这一步比较关键，是此技术的核心，我们举例说明一下：假如现在有一个图片列表，初始有两个图片 `img1，img2`，然后在列表开头加入新图片 `img3，img4`，此时 img1 和 img2 就会被挤到后面去。

假设 img1 的初始位置是 (0, 0)，被挤下去后的位置是 (100, 100)，那么此时浏览器还没有渲染，我们可以在这个时间点通过设置 `img1.style.transform = translate(-100px, -100px)`，让它先 Invert 倒置回位移前的位置。

#### 4. Play

执行动画，前面的准备工作都做好了，最后就是 `Play` 了，移除元素上的 transform（将transform置为0或none） 并启用 tansition 相关的动画。

为了便于大家更好的理解FLIP技术的原理，借用`davidkpiano` 曾在分享中使用的流程图向大家展示，或许更易于理解：
![13c77273c4a440f1a9d94fc30717aca5.jpeg](https://i.loli.net/2020/10/10/h5O9mdYNJZMeCPf.jpg)

![004273d0791e4cffb339c19e09fffe32.gif](https://i.loli.net/2020/10/10/KBQhRV1vPJmca9G.gif)

## 实现

了解了 PLIP 技术的原理后，我们通过实现一个简单的小例子来更好的理解这个概念，实现卡片的增删动画。

首先我们先初始化一个列表如图所示：

![企业咚咚20200929133416.jpg](https://i.loli.net/2020/10/10/cSuOvHpZhA4I3LR.jpg)

然后按照上面四个步骤来开发增删动画。

#### First

此处因为卡片的尺寸在动画过程中不发生任何变化，所以我们只需记录卡片的位置信息即可

``` javascript
let cardIndex = 4
let activeList = null

// 生成初始测试数据
let listData = Array(cardIndex).fill().map((item, index) => ({
  index
}))

// First
activeList.forEach((itemEle, index) => {
  rectInfo = itemEle.getBoundingClientRect()
  transArr[index + stepIndex][0] = rectInfo.left
  transArr[index + stepIndex][1] = rectInfo.top
})
```

#### Last

动画的结束状态，其实就是增加或者删除了卡片之后，其余卡片的位置信息

```
// 0标识增加，1标识删除
let updateType = 0
// 定义新的列表数组
let newListData = null
activeIndex = currentIndex

if (updateType === 0) {
  // 增加卡片
  newListData = this.state.listData.slice(0, activeIndex).concat({
    index: cardIndex++
  }, this.state.listData.slice(activeIndex))
} else {
  // 删除卡片
  newListData = this.state.listData.filter((value, index) => index !== activeIndex)
}
```

#### Invert

获取了动画起始阶段受影响的卡片的位置信息后(newListData)，就可以通过 transform属性对元素的位置进行反转

``` javascript
// 0 增加 1 删除
const updateIndex = updateType === 0 ? 1 : 0
activeList.forEach((item, index) => {
  rect = item.getBoundingClientRect()
  invertArr[index + updateIndex][0] = invertArr[index + updateIndex][0] - rect.left
   invertArr[index + updateIndex][1] = invertArr[index + updateIndex][1] - rect.top
```

#### Play

反转了以后，想让他做动画就简单了

``` javascript
// 生成一个二维数组
function getArrByLen(len) {
  return Array(len).fill().map(() => [0, 0])
}

invertArr = getArrByLen(this.state.listData.length)

<li lassName={`card-item${index >= activeIndex ? ' active' : ''}`} style={index >= activeIndex ? {transform: `translate(${invertArr[index - activeIndex][0]}px, ${invertArr[index - activeIndex][1]}px)`} : null) }>
...主体区域
</li>
```

另外由于card在排列过程中可能出现遮挡问题，此处可以将 `z-index` 初始化为1，之后横排的层级变化，则进行 z-index的提升以获得更好的视觉体验。

```
if (transArr[index + stepIndex][1] !== 0) {
  this.state.listData.some((v, k) => {
    if (index + stepIndex + activeIndex === k) {
      v.zIndex = zIndex++
      return true
    }
    return false
  })
}
```

最后让我们来看一下效果吧~

![2.gif](https://storage.360buyimg.com/imgtools/decec97212-da693e10-0233-11eb-90c1-e1f6821fa5c8.gif)

## FLIP 动画应用场景

FLIP技术适用于动画的初始态或最终态不明确的情况，这时候，使用FLIP非常容易做出动画效果。

例如：渲染一个图片列表，当插入或者删除其中某一个元素项时，它会突然出现或者消失，同时其他兄弟元素项也会瞬移，这对于用户体验非常不好，此时除非你限定死了每个元素项的尺寸以及整体页面的尺寸，否则你无法明确当你任意插入或者删除了某个元素之后，其他元素项应当在什么位置。这里就可以使用FLIP 技术给列表的操作添加过渡动画。

![WX20200929-080630@2x.png](https://i.loli.net/2020/10/10/cEg1OIefohZ8B6N.png)

在创建UI时，添加合理的UI过渡动效，避免跳转和瞬间移动。如果将生活中的一些自然运动用到UI动效中来，将会给你的用户带来眼前一亮的感觉。毕竟，所有与你互动的东西都源自于生活中自然的运动。

但是对于一些你明确的知道它的初始态和结束态的动画，其实你可以直接使用 transform，根本没必要 FLIP，用了反而多此一举。

经总结，FLIP 技术主要应用于以下几个场景：

* 视图之间的过渡
* 图片展开和收缩效果
* 项目删除和添加时填充空白区域的效果
* 网格项的重新排序

## FLIP 能提高动画流畅度

FLIP 最大的优点就是提高动画的流畅度，Google 的 Paul Kinlan 曾做过一个调研 `关于用户对新闻类 APP 最期待什么样的功能`。回答令人惊讶，最多的声音不是离线支持，不是平台间同步，不是更好的通知方式，而是希望使用起来更加平滑。平滑就意味着没有屏幕闪烁，没有卡顿，没有抖动。

用户在网页上进行交互时，比如click，touch等，从交互结束到感知到程序的响应大约需要100ms的生理反应时间。如果你的网站在100ms内作出了相应的响应，那么在用户看来，就是得到了立即的响应。否则用户就会觉得卡顿，反应延迟。

我们可以充分利用用户 100ms 生理反应时间来进行相关的计算：`getBoundingClientRect` 或 `getComputedStyle`，并通过 FLIP 技术使动画尽快开始，最后通过 transform 和 opacity 的动画来保证动画的平滑运行。

![636026-20200917152836815-436837780.jpg](https://i.loli.net/2020/10/10/f23oFhidSa4xBun.jpg)

这是一个用户交互体验的原理图，基于100ms的生理反应时间，我们需要在用户交互后100ms内准备好动画，然后以60fps的帧速进行动画的运行。为什么是60fps，此处不做深入探讨，感兴趣的同学可以去看[《为什么帧率达到60fps页面就流畅？》](https://www.jianshu.com/p/90319dbf6fe7)，这些动画准备计算就是getBoundingClientRect(或getComputedStyle)等的计算，而60fps的真素，则需要通过transform或者opacity的反转动画来实现。我们先将元素设置到了结束位置，通过transform来模拟开始位置，然后去除transform这个反向的过程，使得浏览器知道了动画的开始位置和结束位置，计算量锐减，流畅度自然就提升了。

#### tips

使用 FLIP 技术时有几点需要牢记：

1. 不要超出用户反应时间 100ms。否则用户会觉得你的应用没有立即响应，开发中通过 `DevTools` 注意这一点。
2. 精心组织你的动画。设想一下，如果一个 FLIP 动画正在运行，同时你接着想执行下一个 FLIP 动画，这时就要确保下一个动画的预计算工作是在闲置或用户的反应时间内进行，这样就可以保证两个动画互不影响。
3. 内容可能被扭曲。当进行某些缩放动画时可能导致内容扭曲，毕竟这是一种 Hack 技术。

## 总结

FLIP动画，是一种动画机制，策略，通过反转动画来提高流畅度，其实你在写一些过渡效果的时候，在无意识的情况下用到过，该文章只是将其总结并优化后的产物。另外，强大的 `Vue` 贴心地为我们提供了两个内置组件 `transition` & `transition-group`，其中 `transition-group` 内部已经实现 FLIP 的简单动画队列，方便我们使用，感兴趣的小伙伴可以直接去官网学习使用。

## 参考
[Vue transition-group](https://cn.vuejs.org/v2/guide/transitions.html#%E5%88%97%E8%A1%A8%E8%BF%87%E6%B8%A1)  
[flip-your-animations](https://aerotwist.com/blog/flip-your-animations/)  
[FLIP技术给Web布局带来的变化](https://www.w3cplus.com/javascript/animating-layouts-with-the-flip-technique.html)  