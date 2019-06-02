## 学习[2019 React Redux 完全指南](https://juejin.im/post/5cac8ccd6fb9a068530111c7#heading-29) 笔记

### Redux 的好处

> 数据通过 props 在组件树间向下传递,要想数据向上传递，需要通过回调函数实现，因此必须首先将回调函数向下传递到任何想通过调用它来传递数据的组件中。多级传递数据是一种痛苦,顶级容器组件有一些数据，有一个 4 级以上的子组件需要这些数据。必须要经过一堆并不需要该数据的中间组件。
> 相邻组件间的数据传递,Redux 会为你提供一个可以存储数据的全局 "parent"，然后你可以通过 React-Redux 把兄弟组件和数据 connect 起来。
> 使用 react-redux 的 connect 函数，你可以将任何组件插入 Redux 的 store 以及取出需要的数据。
### Redux 有全局唯一 Store
> redux 给你一个 store，让你可以在里面保存 state，取出 state，以及当 state 发生改变时做出响应。但那就是它所有能做的事。
### Redux Reducer
> 含义：它接收当前 state 和一个 action，然后返回 newState。看起来很像 Array.reduce 里 reducer 的特点！
> 数组reduce
```
它是这样用的：你传入一个函数，遍历数组的每一个元素时都会调用你传入的函数，类似 map 的作用 —— 你可能在 React 里面渲染列表而对 map 很熟悉。
你的函数调用时会接收两个参数：上一次迭代的结果，和当前数组元素。它结合当前元素和之前的 "total" 结果然后返回新的 total 值。
```
### Redux Reducer 和 函数reduce的区别
Redux reducers 就像你传给 Array.reduce 的函数作用一样！:) 它们 reduce 的是 actions。它们把一组 actions（随着时间）reduce 成一个单独的 state。不同之处在于 Array 的 reduce 立即发生，而 Redux 则随着正运行应用的生命周期一直发生。

> reducer 绝不能返回 undefined，惯用的方式是定义一个 initialState 变量然后使用 ES6 默认参数给 state 赋初始值

### Dispatch Actions 来改变 State
> 我们将 "dispatch" 一些 "actions"。
>Actions 的格式非常自由。只要它是个带有 type 属性的对象就可以了。
>为了保证事务的合理性和可维护性，我们 Redux 用户通常给 actions 的 type 属性赋简单字符串，并且通常是大写的，来表明它们是常量。
>为了让 action 做点事情，你需要 dispatch。

### Redux Dispatch 工作机制
> 我们刚才创建的 store 有一个内置函数 dispatch。调用的时候携带 action，Redux 调用 reducer 时就会携带 action（然后 reducer 的返回值会更新 state）。
>每一次调用 dispatch 最终都会调用 reducer！
>为了让 actions 做点事情，我们需要在 reducer 里面写几行代码来根据每个 action 的 type 值来对应得更新 state。

### 如何保持纯 Reducers
>另一个关于 reducers 的规则是它们必须是纯函数。也就是说不能修改它们的参数，也不能有副作用（side effect）。
>副作用（side effect）”是指对函数作用域之外的任何更改。不要改变函数作用域以外的变量，不要调用其他会改变的函数（比如 fetch，跟网络和其他系统有关），也不要 dispatch actions 等。
技术角度来看 console.log 是副作用（side effect），但是我们忽略它。
>不要修改 state 参数,这意味着你不能执行 state.count = 0、state.items.push(newItem)、state.count++ 及其他类型的变动 —— 不要改变 state 本身，及其任何子属性。
>你可以把它想成一个游戏，你唯一能做的事就是 return { ... }。

### Redux规则
- State 是只读的，唯一修改它的方式是 actions。
- 更新的唯一方式：dispatch(action) -> reducer -> new state。
- Reducer 函数必须是“纯”的 —— 不能修改它的参数，也不能有副作用（side effect）

### 如何在 React 中使用 Redux
- 要做到这一点，要用到 react-redux 库的两样东西：一个名为 Provider 的组件和一个 connect 函数。
- 通过用 Provider 组件包装整个应用，如果它想的话，应用树里的每一个组件都可以访问 Redux store。在 index.js 里，引入 Provider 然后用它把 App 的内容包装起来。store 会以 prop 形式传递。

### React-Redux Provider 工作机制
- Provider 可能看起来有一点点像魔法。它在底层实际是用了 React 的 Context 特性。Context 就像是连接每个组件的秘密通道，使用 connect 就可打开秘密通道的大门。

### 为 Redux 准备 Counter 组件
- 这样写是因为 connect 是一个高阶函数，它简单说就是当你调用它时会返回一个函数。然后调用返回的函数传入一个组件时，它会返回一个新（包装的）组件。
- Connect 做的是在 Redux 内部 hook，取出整个 state，然后把它传进你提供的 mapStateToProps 函数。它是个自定义函数，因为只有你知道你存在 Redux 里面的 state 的“结构”。
### mapStateToProps 工作机制
- mapStateToProps 返回的对象以 props 形式传给了你的组件
- 对象的属性变成了 prop 名称，它们对应的值会变成 props 的值。你看，这个函数就像字面含义一样定义从 state 到 props 的映射。

### 为什么不传整个 state？
- 在很小的例子中，可能会传全部 state，但通常你只会从更大的 state 集合中选择部分组件需要的数据。并且，没有 mapStateToProps 函数，connect 不会传递任何 state。你可以传整个 state，然后让组件梳理。但那不是一个很好的习惯，因为组件需要知道 Redux state 的结构然后从中挑选它需要的数据，后面如果你想更改结构会变得更难。

### 从 React 组件 Dispatch Redux Actions
- connect 为你提供支持：除了传递（mapped）state，它还从 store 传递了 dispatch 函数!
- 要在 Counter 内部 dispatch action，我们可以调用 this.props.dispatch 携带一个 action。
