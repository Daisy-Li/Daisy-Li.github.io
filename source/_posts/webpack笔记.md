---
title: webpack笔记
tags: 
  - 技术
  - 构建工具
categories:
  - 前端
  - 经验沉淀
keywords: "webpack 前端构建"
date: 2021-12-03 15:00:00
top_img: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/webpacklegao.jpeg
cover: https://cdn.jsdelivr.net/gh/Daisy-Li/image-cloud/webpacklegao.jpeg
---

### webpack核心概念：
- **entry** 入口 一般是src/index.js
- **output** webpack的输出 最终构建出来的静态文件
- **loader** 模块代码转换的工作由 loader 来处理 loader是一个转换器，负责把某种文件格式的内容转换成 webpack 可以支持打包的模块。
- **plugin** 插件 plugin 用于处理更多其他的一些构建任务
- **mode** 构建模式

### webpack开发环境最基础的构建需求

- 构建我们发布需要的 HTML、CSS、JS 文件
- 使用 CSS 预处理器来编写样式
- 引用图片
- 使用 Babel 来支持 ES 新特性
- 本地提供静态服务以方便开发调试

### webpack5 css 背景图片无法加载问题的解决思路：
webpack5 增加了asset mudule（资源模块）  
https://blog.csdn.net/Coralpapy/article/details/119419137

webpack的初衷是让js支持模块化管理。所谓 webpack 构建的起点，本质上也是一个 module，而我们在设置好 webpack 后，开发的过程亦是在写一个个的业务 module。

### webpack 路径解析 enhanced-resolve模块

-   解析相对路径

    1.  查找相对当前模块的路径下是否有对应文件或文件夹
    2.  是文件则直接加载
    3.  是文件夹则继续查找文件夹下的 package.json 文件
    4.  有 package.json 文件则按照文件中 `main` 字段的文件名来查找文件
    5.  无 package.json 或者无 `main` 字段则查找 `index.js` 文件

-   解析模块名  
    查找当前文件目录下，父级目录及以上目录下的 `node_modules` 文件夹，看是否有对应名称的模块

-   解析绝对路径（不建议使用）  
    直接查找对应路径的文件
    
在 webpack 配置中，和模块路径解析相关的配置都在 `resolve` 字段下


### resolve 配置

#### `resolve.alias`

路径别名：

```
"@": paths.appSrc,  
"@components": paths.appComponent,  
"@pages": paths.appPage,  
```

#### `resolve.extensions`
路径补全：

```
extensions: ['.wasm', '.mjs', '.js', '.json', '.jsx'], 
// 这里的顺序代表匹配后缀的优先级，例如对于 index.js 和 index.jsx，会优先选择 index.js
```

### loader

由于 loader 处理的是我们代码模块的内容转换，所以都是在 `module.rules` 中添加新的配置规则，在该字段中，每一项被视为一条匹配使用 loader 的规则。

test,include,exclude,use,type，enforce（指定顺序）关键配置项

- 同一规则顺序：从后到前
- 不同规则顺序：**前置 -> 行内 -> 普通 -> 后置**

webpack 的 loader 相关配置都在 `module.rules` 字段下，我们需要通过 `test`、`include`、`exclude` 等配置好应用 loader 的条件规则，然后使用 `use` 来指定需要用到的 loader，配置应用的 loader 时还需要注意一下 loader 的执行顺序。

#### loader 是一个 function

接收模块代码的内容，然后返回代码内容转化后的结果

-   最后的 loader 最早调用，传入原始的资源内容（可能是代码，也可能是二进制文件，用 buffer 处理）
-   第一个 loader 最后调用，期望返回是 JS 代码和 sourcemap 对象（可选）
-   中间的 loader 执行时，传入的是上一个 loader 执行的结果


### plugin

#### DefinePlugin

DefinePlugin 是 webpack 内置的插件，可以使用 `webpack.DefinePlugin` 直接获取，前边也提过，在不同的 mode 中，会使用 DefinePlugin 来设置运行时的 `process.env.NODE_ENV` 常量。  
社区中关于 DefinePlugin 使用得最多的方式是定义环境常量。

#### TerserPlugin

webpack mode 为 production 时会启用 TerserPlugin 来压缩 JS 代码

### 一个简单的 plugin

plugin 的实现可以是一个类，使用时传入相关配置来创建一个实例，而 plugin 实例中最重要的方法是 `apply`，该方法在 webpack compiler 安装插件时会被调用一次，`apply` 接收 webpack compiler 对象实例的引用，你可以在 compiler 对象实例上注册各种事件钩子函数，来影响 webpack 的所有构建流程，以便完成更多其他的构建任务。

事件钩子可以理解为当 webpack 运行中执行到某个钩子的状态时，便会触发你注册的事件，即发布订阅模式。
关于 webpack hooks 底层的实现，其实都是基于 webpack 作者开发的 [tapable](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fwebpack%2Ftapable%2F "https://github.com/webpack/tapable/") 这个库提供的钩子函数。

### 优化

#### 图片优化

1. image-webpack-loader
2. url-loader limit属性，小于一定值则转换为base64编码的DataURL

#### html压缩 

HtmlWebpackPlugin minify属性

#### css压缩

MiniCssExtractPlugin

#### js优化

1. mode选择production 即可默认开启各种 JS 代码压缩优化的配置。
2. Tree shaking 
3. sideEffects
4. concatenateModules 

tree shaking 依赖于 ES2015 模块系统中的[静态结构特性](https://link.juejin.cn/?target=http%3A%2F%2Fexploringjs.com%2Fes6%2Fch_modules.html%23static-module-structure "http://exploringjs.com/es6/ch_modules.html#static-module-structure")，可以移除 JavaScript 上下文中的未引用代码，删掉用不着的代码，能够有效减少 JS 代码文件的大小。


sideEffects 是什么呢？我用一句话来概括就是：让 webpack 去除 tree shaking 带来副作用的代码。

webpack 的 tree shaking 依赖于 babel编译 + UglifyJS 压缩，这个过程是没有完善的程序流分析，UglifyJS 没有完善的程序流分析。它可以简单的判断变量后续是否被引用、修改，但是不能判断一个变量完整的修改过程，不知道它是否已经指向了外部变量，所以很多有可能会产生副作用的代码，都只能保守的不删除。

解释：https://zhuanlan.zhihu.com/p/41795312

### 分离代码文件

`optimization.splitChunks` 配置来拆分代码文件的技巧，包括抽离公共部分代码

按需加载异步代码模块：

``` javascript
// import 作为一个方法使用，传入模块名即可，返回一个 promise 来获取模块暴露的对象 
// 注释 webpackChunkName: "jquery" 可以用于指定 chunk 的名称，在输出文件时有用 
import(/* webpackChunkName: "jquery" */ 'jquery').then(($) => { 
console.log($); 
});
```

### webpack-dev-server 快速启动一个静态服务

- public 字段用于指定静态服务的域名 默认是 http://localhost:8080/
- port 字段用于指定静态服务的端口 默认8080
- publicPath 字段用于指定构建好的静态文件在浏览器中用什么路径去访问，默认是 `/`
- proxy 用于配置 webpack-dev-server 将特定 URL 的请求代理到另外一台服务器上
- contentBase 用于配置提供额外静态文件内容的目录

建议将 `devServer.publicPath` 和 `output.publicPath` 的值保持一致。

#### HMR

hot: true

首先我们要知道一个概念：webpack 内部运行时，会维护一份用于管理构建代码时各个模块之间交互的表数据，webpack 官方称之为 **Manifest**，其中包括入口代码文件和构建出来的 bundle 文件的对应关系。


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f898ae316ac64fa98c9f694d6f19da2f~tplv-k3u1fbpfcp-watermark.image?)

### 开发流程

区分开发环境：

webpack.config.js：

``` js
module.exports = (env, argv) => ({ 
    mode: env.production ? 'production' : 'development', // 从 env 参数获取 mode 
    devtool: env.production ? false : 'eval-cheap-source-map', // 开发环境需要 source map })
```

package.json：

``` json
{
  "scripts": {
    "build:pro": "webpack --env.production"
  }
}

```

拆分文件，使用`webpack-merge`插件

-   webpack.base.js：基础部分，即多个文件中共享的配置
-   webpack.development.js：开发环境使用的配置
-   webpack.production.js：生产环境使用的配置
-   webpack.test.js：测试环境使用的配置

``` js
const WebPackMerge = require('webpack-merge')
const webpack = require('webpack') 
const base = require('./webpack.base.js') 

module.exports = WebPackMerge(base, { 
    module: { 
        rules: [ 
            // 用 WebPackMerge API，当这里的匹配规则相同且 use 值都是数组时，smart 会识别后处理 
            // 和上述 base 配置合并后，这里会是 { test: /\.js$/, use: ['babel', 'coffee'] } 
            // 如果这里 use 的值用的是字符串或者对象的话，那么会替换掉原本的规则 use 的值 
            { 
                test: /\.js$/, 
                use: ['coffee'], 
            }, 
            // ... 
        ], 
   }, 
   plugins: [ 
       // plugins 这里的数组会和 base 中的 plugins 数组进行合并 
       new webpack.DefinePlugin({ 
           'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV), 
       }), 
   ], 
})
```

package.json

``` json
"scripts": {
    "serve": "webpack-dev-server --config config/webpack.config.dev.js --mode development --colors",
    "build:yufa": "rm -rf deploy/* && webpack --config config/webpack.config.beta.js --mode development --colors --progress",
    "build": "rm -rf deploy/* && webpack --config config/webpack.config.prod.js --mode production --colors --progress",
  },
```

### create-react-app

- eject
- react-app-rewired

### webpack-chain

@vue/cli 提供了一种基于链式 api 的 webpack 配置方式，并且它内部的 webpack 配置也是用这种方式维护的，而这种配置方式的核心类库就是 [webpack-chain](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fneutrinojs%2Fwebpack-chain "https://github.com/neutrinojs/webpack-chain")。


### wenpack 工作原理

webpack 本质上就是一个 JS 模块 Bundler，用于将多个代码模块进行打包。

#### 合并代码

主要解决两个问题：

1. 文件合并时的顺序很难确定 （模块依赖树）
2. 代码文件内变量和方法命名容易冲突 (模块化)

在已经解析出依赖关系的前提下，webpack 会利用 JavaScript Function 的特性提供一些代码来将各个模块整合到一起，即是将每一个模块包装成一个 JS Function，提供一个引用依赖模块的方法，如下面例子中的 `__webpack__require__`，这样做，既可以避免变量相互干扰，又能够有效控制执行顺序，(生产环境中可以用唯一的模块 id 去调整模块内变量名防止冲突)简单的代码例子如下：

``` js
modules['./entry.js'] = function() { 
    const { bar } = __webpack__require__('./bar.js') 
}
```

### webpack结构

-   Compiler，webpack 的运行入口，实例化时定义 webpack 构建主要流程，同时创建构建时使用的核心对象 compilation
-   Compilation，由 Compiler 实例化，存储构建过程中各流程使用到的数据，用于控制这些数据的变化
-   Chunk，即用于表示 chunk 的类，即构建流程中的主干，一般情况下一个入口会对应一个 chunk，对于构建时需要的 chunk 对象由 Compilation 创建后保存管理
-   Parser，其中相对复杂的一个部分，基于 [acorn](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Facornjs%2Facorn "https://github.com/acornjs/acorn") 来分析 AST 语法树，解析出代码模块的依赖
-   Module，用于表示代码模块的类，衍生出很多子类用于处理不同的情况，关于代码模块的所有信息都会存在 Module 实例中，例如 `dependencies` 记录代码模块的依赖等
-   Dependency，解析时用于保存代码模块对应的依赖使用的对象
-   Template，生成最终代码要使用到的代码模板，像上述提到的 function 代码就是用对应的 Template 来生成

#### 工作流程

```
创建 Compiler -> 
调用 compiler.run 开始构建 -> 
创建 Compilation -> 
基于配置开始创建 Chunk -> 
使用 Parser 从 Chunk 开始解析依赖 -> 
使用 Module 和 Dependency 管理代码模块相互关系 -> 
使用 Template 基于 Compilation 的数据生成结果代码 ->
```

### 提升构建速度

提升 webpack 构建速度本质上就是想办法让 webpack 少干点活，活少了速度自然快了，尽量避免 webpack 去做一些不必要的事情，记得这个主要方向，后续的针对构建速度的优化都是围绕着这一方向展开。

#### 配置优化

1. 减少 `resolve` 的解析 

``` js
resolve: {
  modules: [
    path.resolve(__dirname, 'node_modules'), // 使用绝对路径指定 node_modules，不做过多查询
  ],

  // 删除不必要的后缀自动补全，少了文件后缀的自动匹配，即减少了文件路径查询的工作
  // 其他文件可以在编码时指定后缀，如 import('./index.scss')
  extensions: [".js"], 

  // 避免新增默认文件，编码时使用详细的文件路径，代码会更容易解读，也有益于提高构建速度
  mainFiles: ['index'],
},
```

2. 把 loader 应用的文件范围缩小

我们在使用 loader 的时候，尽可能把 loader 应用的文件范围缩小，只在最少数必须的代码模块中去使用必要的 loader，例如 node_modules 目录下的其他依赖类库文件，基本就是直接编译好可用的代码，无须再经过 loader 处理了：

``` js
rules: [
    { 
        test: /\.jsx?/, 
        include: [ path.resolve(__dirname, 'src'), 
        // 限定只在 src 目录下的 js/jsx 文件需要经 babel-loader 处理 
        // 通常我们需要 loader 处理的文件都是存放在 src 目录 ], 
        use: 'babel-loader', 
    }, 
    // ... 
],
```

3. 减少 plugin 的消耗

区分mode

4. 选择合适的 devtool

在构建生产环境代码时不输出 sourcemap，而开发环境时一般选用 `eval-cheap-source-map` 来确保 sourcemap 基本可用的情况下还有着不错的构建速度。


![A.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ab309339c9c4131842cb68822f4d447~tplv-k3u1fbpfcp-watermark.image?)
