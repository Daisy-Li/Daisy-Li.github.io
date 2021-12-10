---
title: React组件中向props.children传递数据
tags: 
  - 技术
  - react
categories:
  - 前端
  - react
keywords: "react 组件通信"
date: 2021-12-08 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/meitu.jpeg
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/meitu.jpeg
---

我们都知道在 `React` 组件中向子组件传递数据很容易，但是如何向 `props.children` 传递数据呢？

最近在做一个倒计时组件，就涉及到向 `props.children()`传参，实际上`ButtonGroup`/`CheckboxGroup` 等组件也时常会遇到相同的情况，所以今天我们来探究一下`props.children()`。

### 向子组件传参

将需要传递的数据放到props里面即可，例如：

``` jsx
<div className={styles.header}>
    <HeadSwiper bannerList={productImgList}/>
</div>
```

上面示例就是简单的向`HeadSwiper`传递图片列表数据。


### 通过props.children()传参

这里以倒计时组件为例，countDown组件需要将计算后的时分秒传给他的子元素，子元素通过一个回调函数获取这个参数，如下所示：

``` jsx
// layout

const renderCountDown = useCallback(
    ({ dayNum, dayStr, hourStr, minuteStr, secondStr }) => {
      return (
        <RenderCountDown
          auctionStatus={auctionStatus}
          dayNum={dayNum}
          dayStr={dayStr}
          hourStr={hourStr}
          minuteStr={minuteStr}
          secondStr={secondStr}
        />
      )
    },
    [auctionStatus]
)

<CountDown
  className={styles['countdown-container']}
  leftTime={remainTime}
  onEnd={props.handleCountDownDone}
>
  {renderCountDown}
</CountDown>
```

那个countdown组件里面又是如何将`dayNum, dayStr, hourStr, minuteStr, secondStr`这几个参数传出来的，让我们看下具体实现：

``` jsx

/**
 * @description: 格式化剩余时间
 * @param {type} 剩余时间
 * @return: rest对象
 */
const formatRestTime = (t) => {
  const ts = t
  const rest = {
    dayStr: '00',
    hourStr: '00',
    minuteStr: '00',
    secondStr: '00',
    dayNum: 0,
    hourNum: 0,
    minuteNum: 0,
    secondNum: 0
  }
  // ...
  return rest
}

function CountDown (props) {
  const { leftTime, onEnd } = props
  const [restTimeObj, setRestTimeObj] = useState(() => formatRestTime(Math.round(leftTime / 1000)))
   // ...
  return <div className={props.className}>{props.children(restTimeObj)}</div>
}

export default React.memo(CountDown)

```

countdown组件里面使用`props.children(restTimeObj)`的方式将`restTimeObj`对象传给了他的children。

### 总结

记录一种参数传递的方式，但是我在官网没有找到相关操作的文档，如果有小伙伴对这一部分比较熟悉，欢迎留言讨论，作者自己其实也没太学习明白，太难了~ 😭
