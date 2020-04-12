### js的模块化
  ```
    在ES6规范以前，我们有commonjs和AMD等主流模块化
  ```
###  commonjs
![111](../../../image/webpack/webpack-01.png)

```
moduleB.js

module.exports = “ hello world”

```

```
index.js
 const str = require('./moduleB');
 console.log(str)
```

```
moduleA.js // modlueA引入了moduleB

const timestamp = require('./moduleB')
console.log('moduleA', timestamp)
```
***js是没有模块化的，再node出来之后引入了commonjs,commmonjs有require和module.exports()对象，但是它是类似单例模式的。同步进行的，打印的时间戳是一样的。同一个模块加载多少次，结果都是一样的，都是第一次的缓存***
![111](../../../image/webpack/webpack-02.png)

### AMD
![111](../../../image/webpack/webpack-03.png)
***define进行模块的定义，如果不手动定义id的话，那么默认就是文件名作为id.require是异步加载，所以写在回调里面返回结果，同一个模块加载多少次，结果都是一样的，都是第一次的缓存。由此可见AMD是将所有的模块，在模块执行之前，就全部加载完毕了***
```
moduleB.js

define(function(){
  return new Date.getTime()
})
```

```
moduleA.js

define(function(require){
  const timestamp = require('moduleB')
  console.log(timestamp)
})
```
```
// require是异步加载，所以写在回调里面返回结果
index.js 

require(['moduleA','moduleB'],function(moduleA，moduleB){
  console.log("moduleB")

})

```
![111](../../../image/webpack/webpack-04.png)

***require.js的原理是，使用jsonp实现一个异步加载并执行回调***
### ESModule

```
moduleB.js

var m = new Date().getTime();
export default m;
```

```
moduleA.js

import timestamp from './moduleB'
console.log('moduleB',timetamp)
```

```
index.js 

import './moduleA';
import timestamp from './moduleB'
console.log('index.js',timestamp)
```
![111](../../../image/webpack/webpack-05.png)

- 每个 JS 的运行环境都有一个解析器，否则这个环境也不 会认识 JS 语法。它的作用就是用 ECMAScript 的规范去 解释 JS 语法，也就是处理和执行语言本身的内容，例如 按照逻辑正确执行 var a = "123";，function func() {console.log("hahaha");} 之类的内容。
在解析器的上层，每个运行环境都会在解释器的基础上 封装一些环境相关的 API。例如 Node.js 中的 global 对象、process 对象，浏览器中的 window 对 象，document 对象等等。这些运行环境的 API 受到各自规范的影响，例如浏览器端的 W3C 规范，它们规定 了 window 对象和 document 对象上的 API 内容，以使 得我们能让 document.getElementById 这样的 API 在 所有浏览器上运行正常。

![111](../../../image/webpack/webpack-06.png)

- JS Core 层面的规范
```
ESModule 就属于 JS Core 层面的规范，而 AMD， CommonJS 是运行环境的规范。所以，想要使运行环境 支持 ESModule 其实是比较简单的，只需要升级自己环 境中的 JS Core 解释引擎到足够的版本，引擎层面就能 认识这种语法，从而不认为这是个 语法错误(syntax error) ，运行环境中只需要做一些兼容工作即可。
Node.js 在 V12 版本之后才可以使用 ESModule 规范的 模块，在 V12 没进入 LTS 之前，我们需要加上 -- experimental-modules 的 flag 才能使用这样的特 性，也就是通过 node --experimental-modules，index.js 来执行。浏览器端 Chrome 61 之后的版本可 以开启支持 ESModule 的选项，只需要通过 `` 这样的 标签加载即可。
这也就是说，如果想在 Node.js 环境中使用 ESModule，就需要升级 Node.js 到高版本，这相对来 说比较容易，毕竟服务端 Node.js 版本控制在开发人员 自己手中。但浏览器端具有分布式的特点，是否能使用 这种高版本特性取决于用户访问时的版本，而且这种解 释器语法层面的内容无法像 AMD 那样在运行时进行兼 容，所以想要直接使用就会比较麻烦。
```
- 后模块化的编译时代
```
  通过前面的分析我们可以看出来，使用 ESModule 的模 块明显更符合 JS 开发的历史进程，因为任何一个支持 JS 的环境，随着对应解释器的升级，最终一定会支持 ESModule 的标准。但是，WEB 端受制于用户使用的浏 览器版本，我们并不能随心所欲的随时使用 JS 的最新特 性。为了能让我们的新代码也运行在用户的老浏览器 中，社区涌现出了越来越多的工具，它们能静态将高版 本规范的代码编译为低版本规范的代码，最为大家所熟 知的就是 babel。
     
它把 JS Core 中高版本规范的语法，也能按照相同语义 在静态阶段转化为低版本规范的语法，这样即使是早期 的浏览器，它们内置的 JS 解释器也能看懂。

然后，不幸的是，对于模块化相关的 import 和 export 关键字，babel 最终会将它编译为包含 require 和 exports 的 CommonJS 规范。
这就造成了另一个问题，这样带有模块化关键词的模 块，编译之后还是没办法直接运行在浏览器中，因为浏 览器端并不能运行 CommonJS 的模块。为了能在 WEB 端直接使用 CommonJS 规范的模块，除了编译之外， 我们还需要一个步骤叫做打包(bundle)。

打包工具的作用，就是将模块化内部实现的细节抹平， 无论是 AMD 还是 CommonJS 模块化规范的模块，经过 打包处理之后能变成能直接运行在 WEB 或 Node.js 的 内容。
```

- 如何处理打包

```
参考 Node.js 源码来熟悉
CommonJS 的处理方式
我们可以参考 CommonJS 模块的处理方式来处理一个 CommonJS 模块。在 node.js 中，所有的 CommonJS 模块都会被包裹在一个函数中，然后在 node.js 中使用 vm 来运行它，最终达到一个模块化导入和导出的目 的。
```

- 浏览器中对 CommonJS 处理
```

我们在浏览器中也可以按照相同的思路来进行处理，我
们在打包阶段将每个模块包裹上一层函数字符串，然后
放置到浏览器中去执行它。
同时我们实现一个简单版本的 require 函数和 module 对象，来处理运行时加载的问题。
 
这样一个基本的流程就做好了，接下来我们要处理的就
是运行时的依赖关系，我们需要运行时明白模块之间的
依赖关系，所以我们需要自己维护一个配置，运行时来
进行查找。
有了查找关系之后，我们就可以在运行时注入这部分内
容，获取内容的时候通过这部分配置来拿到模块之间的
映射关系。
```
- 异步组件打包

```
上面我们说到的都是同步组件的打包，最终所有的组件
都同步的打包进同一个文件当中，但有的时候我们需要
将组件进行异步加载，异步加载的过程中，我们的组件
需要不在主包当中，而在其他的子包文件当中。
这时候，我们就需要使用另外的异步策略来处理，我们 这里采用 jsonp 的原理来执行。

```

- HMR 原理

```
明白了同步打包和异步打包之后，我们的工具基本上就 已经覆盖了大部分功能，那我们如何进行 hot module reload 呢?这里简单给大家讲解一下核心的原理。
hot module reload 就是当我们对文件内容有改动的时 候，就会重新触发编译，同时也会重新触发 UI 的更新， 达到了我们无须重新更新打包就能更新我们的应用。
我们只需要清除我们函数内部的缓存和模块的代码，这
样不刷新⻚面的情况下，只需要重新加载组件就能达到
效果。
```
- [JS MODULE 大战](https://juejin.im/post/5cb74b73e51d456e577f935c)