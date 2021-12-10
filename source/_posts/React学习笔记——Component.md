---
title: React学习笔记——Component
tags: 
  - react
categories:
  - 学习笔记
  - React
keywords: "react component"
date: 2021-12-09 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/react.webp
---

在 React 世界里，一切皆组件，我们写的 React 项目全部起源于组件。组件可以分为两类，一类是类（ Class ）组件，一类是函数（ Function ）组件。

### React组件结构

``` js
/* 类 */ 
class testClass { 
    sayHello=()=>console.log('hello, world') 
} 

/* 类组件 */
export default class FreshHot extends React.PureComponent<Partial<any>> { 
    state={ message:`hello ，world!` } 
    sayHello=()=> this.setState({ message : 'hello, change' }) 
    
    componentDidMount() {
        // 初始化
    }
    
    public render(){ 
        return <div onClick={ this.sayHello } > { this.state.message } </div> 
    } 
} 

/* 函数 */ 
function testFunc (){ 
    return 'hello, world' 
} 

/* 函数组件 */ 
function FuncComponent(props){ 
    const { dataList } = props
    const [ message , setMessage ] = useState('hello,world') 
    return <div onClick={ ()=> setMessage('hello, change') } >{ message }</div> 
}
```

组件本质上就是类和函数，但是与常规的类和函数不同的是，**组件承载了渲染视图的 UI 和更新视图的 setState 、 useState 等方法**

react组件是在 React调和渲染fiber节点的时候分别被实例化或执行的，分别有对用的脚本去执行，具体可以关注`react-reconciler`。

### 类组件

生命周期：

``` js
class App extends React.Component {
	constructor(props) {
		super(props);
		console.log("constructor")

		this.onClickHandler = this.onClickHandler.bind(this);
	}
	componentWillMount() {
		console.log("componentWillMount")
	}
	componentDidMount() {
		console.log("componentDidMount")
	}
	componentWillUnmount() {
		console.log("componentWillUnmount")
	}
	componentWillReceiveProps() {
		console.log("componentWillReceiveProps")
	}
	shouldComponentUpdate() {
		console.log("shouldComponentUpdate")
		return true
	}
	componentWillUpdate() {
		console.log("componentWillUpdate")
	}
	componentDidUpdate() {
		console.log("componentDidUpdate")
	}

	onClickHandler() {
		console.log("onClickHandler")
		this.forceUpdate();
	}

	render() {
		console.log("render")
		return (
			<button onClick={this.onClickHandler}> click here </button>
		);
	}
}

ReactDOM.render(<App />,
	document.getElementById("react-container")
);
```

在 class 组件中，除了继承 React.Component ，底层还加入了 updater 对象，类组件执行构造函数过程中会在实例上绑定 props 和 context ，初始化置空 refs 属性，原型链上绑定setState、forceUpdate 方法。

### 函数组件

函数组件直接调用执行，不会创建实例，原型上的属性方法无效。依靠hook实现转态存储能力。

### 选择函数组件的原因

函数组件：纯函数，输入props,输出jsx，没有实例，没有this，没有state，没有生命周期，hook出现就不一样了，函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。为了提高性能，尽量使用函数组件。函数组件代码更简洁，性能很好；

### 组件通信

React 一共有 5 种主流的通信方式：

1.  props 和 callback 方式
2.  ref 方式。
3.  React-redux 或 React-mobx 状态管理方式。
4.  context 上下文方式。
5.  event bus 事件总线。

props 和 callback 可以作为 React 组件最基本的通信方式，父组件可以通过 props 将信息传递给子组件，子组件可以通过执行 props 中的回调函数 callback 来触发父组件的方法，实现父与子的消息通讯。

### 增强组件的方式

1. 类继承
2. 自定义 hooks
3. 高阶组件 hoc