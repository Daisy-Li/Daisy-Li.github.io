---
title: React学习笔记——State
tags: 
  - react
categories:
  - 学习笔记
  - React
keywords: "react state"
date: 2021-12-10 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
---

### state到底是同步还是异步的

batchUpdate 批量更新概念，以及批量更新被打破的条件。


### 类组件 setState

```
setState(obj,callback)
```

- 第一个参数：当 obj 为一个对象，则为即将合并的 state ；如果 obj 是一个函数，那么当前组件的 state 和 props 将作为参数，返回值用于合并新的 state。
- 第二个参数 callback ：callback 为一个函数，函数执行上下文中可以获取当前 setState 更新后的最新 state 的值，可以作为依赖 state 变化的副作用函数，可以用来做一些基于 DOM 的操作。

``` js
/* 第一个参数为function类型 */ 
this.setState((state,props)=>{ 
    return { number:1 } 
}) 
/* 第一个参数为object类型 */ 
this.setState({ number:1 },()=>{ 
    console.log(this.state.number) //获取最新的number 
})
```

#### state更新过程

- 首先 setState会先确定当前更新优先级；（flushSync 中的 setState  **>**  正常执行上下文中 setState  **>**  setTimeout ，Promise 中的 setState。）
- render阶段：react会从fiber根节点向下调和子节点，调和阶段会对比发生更新的地方，更新对比expirationTime，找到发生更新的组件，合并state，执行render函数，得到新的UI视图层；
- commit阶段：替换真实的Dom,然后执行setState中的回调函数。

#### 类组件如何限制 state 更新视图

- pureComponent 可以对 state 和 props 进行浅比较，如果没有发生变化，那么组件不更新
- shouldComponentUpdate 生命周期可以通过判断前后 state 变化来决定组件需不需要更新，需要更新返回true，否则返回false。

#### setState 底层原理

实际上是 React 底层调用 Updater 对象上的 enqueueSetState 方法，创建一个 update ，然后放入当前 fiber 对象的待更新队列中，最后开启调度更新，进入上述讲到的更新流程。

updater是一个常量对象，不能更改，由const声明。

``` js
enqueueSetState(){ 
    /* 每一次调用`setState`，react 都会创建一个 update 里面保存了 */ 
    const update = createUpdate(expirationTime, suspenseConfig); 
    /* callback 可以理解为 setState 回调函数，第二个参数 */ 
    callback && (update.callback = callback) 
    /* enqueueUpdate 把当前的update 传入当前fiber，待更新队列中 */ 
    enqueueUpdate(fiber, update); 
    /* 开始调度更新 */ 
    scheduleUpdateOnFiber(fiber, expirationTime); 
}
```

#### batch update 批量更新

正常 **state 更新**、**UI 交互**，都离不开用户的事件，比如点击事件，表单输入等，React 是采用事件合成的形式，每一个事件都是由 React 事件系统统一调度的，那么 State 批量更新正是和事件系统息息相关的。
同步代码里面会出现批量更新的情况，但是在异步操作里面的批量更新规则会被打破，比如用 promise 或者 setTimeout 包裹的代码块。

React-Dom 中提供了批量更新方法 `unstable_batchedUpdates`，用于在异步环境继续开启批量更新。

在实际工作中，unstable_batchedUpdates 可以用于 Ajax 数据交互之后，合并多次 setState，或者是多次 useState 。原因很简单，所有的数据交互都是在异步环境下，如果没有批量更新处理，一次数据交互多次改变 state 会促使视图多次渲染。

### 函数组件中的useState

```
[ ①state , ②dispatch ] = useState(③initData)
const [ number , setNumber ] = React.useState(0)
```

- state，目的提供给 UI ，作为渲染视图的数据源。
- dispatch 改变 state 的函数，可以理解为推动函数组件渲染的渲染函数。
- initData 有两种情况，第一种情况是非函数，将作为 state 初始化的值。 第二种情况是函数，函数的返回值作为 useState 初始化的值。

**dispatch 参数是一个非函数值**

``` js
const [ number , setNumbsr ] = React.useState(0)
/* 一个点击事件 */
const handleClick=()=>{
   setNumber(1)
   setNumber(2)
   setNumber(3)
}
```

**dispatch 参数是一个函数**

``` js
const [ number , setNumbsr ] = React.useState(0)
const handleClick=()=>{
   setNumber((state)=> state + 1)  // state - > 0 + 1 = 1
   setNumber(8)  // state - > 8
   setNumber((state)=> state + 1)  // state - > 8 + 1 = 9
}
```

#### useEffect 监听state变化

注意：useEffect 初始化会默认执行一次。

当调用改变 state 的函数dispatch，在本次函数执行上下文中，是获取不到最新的 state 值的

``` js
const [ number , setNumber ] = React.useState(0) 
const handleClick = ()=>{ 
    ReactDOM.flushSync(()=>{ 
        setNumber(2) 
        console.log(number) 
    }) 
    setNumber(1) 
    console.log(number) 
    setTimeout(()=>{ 
        setNumber(3) 
        console.log(number) 
    }) 
}
```
执行结果 0 0 0

原因很简单，函数组件更新就是函数的执行，在函数一次执行过程中，函数内部所有变量重新声明，所以改变的 state ，只有在下一次函数组件执行时才会被更新。所以在如上同一个函数执行上下文中，number 一直为0，无论怎么打印，都拿不到最新的 state。

### 类组件中的 `setState` 和函数组件中的 `useState` 有什么异同

相同点：
- 首先从原理角度出发，setState和 useState 更新视图，底层都调用了 scheduleUpdateOnFiber 方法，而且事件驱动情况下都有批量更新规则。

不同点：

-   在不是 pureComponent 组件模式下， setState 不会浅比较两次 state 的值，只要调用 setState，在没有其他优化手段的前提下，就会执行更新。但是 useState 中的 dispatchAction 会默认比较两次 state 是否相同，然后决定是否更新组件。
-   setState 有专门监听 state 变化的回调函数 callback，可以获取最新state；但是在函数组件中，只能通过 useEffect 来执行 state 变化引起的副作用。
-   setState 在底层处理逻辑上主要是和老 state 进行合并处理，而 useState 更倾向于重新赋值。