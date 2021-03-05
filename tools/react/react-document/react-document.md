### React是什么

1. 使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

    ```
    class ShoppingList extends React.Component {
            render() {
                return (
                <div className="shopping-list">
                    <h1>Shopping List for {this.props.name}</h1>
                    <ul>
                    <li>Instagram</li>
                    <li>WhatsApp</li>
                    <li>Oculus</li>
                    </ul>
                </div>
                );
            }
            }

        // 用法示例: <ShoppingList name="Mark" />
    ```
<br>
- 其中，ShoppingList 是一个 React 组件类，或者说是一个 React 组件类型。一个组件接收一些参数，我们把这些参数叫做 props（“props” 是 “properties” 简写），然后通过 render 方法返回需要展示在屏幕上的视图的层次结构。

- 更具体地来说，render 返回了一个 React 元素，这是一种对渲染内容的轻量级描述。大多数的 React 开发者使用了一种名为 “JSX” 的特殊语法，JSX 可以让你更轻松地书写这些结构。语法 ```<div /> ```会被编译成 React.createElement('div')。

```
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

## [react文档学习](https://react.docschina.org/docs/hooks-intro.html)

### React是什么

1. 使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

    ```
    class ShoppingList extends React.Component {
            render() {
                return (
                <div className="shopping-list">
                    <h1>Shopping List for {this.props.name}</h1>
                    <ul>
                    <li>Instagram</li>
                    <li>WhatsApp</li>
                    <li>Oculus</li>
                    </ul>
                </div>
                );
            }
            }

        // 用法示例: <ShoppingList name="Mark" />
    ```
<br>

- 其中，ShoppingList 是一个 React 组件类，或者说是一个 React 组件类型。一个组件接收一些参数，我们把这些参数叫做 props（“props” 是 “properties” 简写），然后通过 render 方法返回需要展示在屏幕上的视图的层次结构。

- 更具体地来说，render 返回了一个 React 元素，这是一种对渲染内容的轻量级描述。大多数的 React 开发者使用了一种名为 “JSX” 的特殊语法，JSX 可以让你更轻松地书写这些结构。语法 ```<div /> ```会被编译成 React.createElement('div')。

```
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

### react文档学习

- react 子向父传递事件

    -  Board 组件向 Square 组件中传递两个 props 参数：value 和 onClick
    -  每一个 Square 被点击时，Board 提供的 onClick 函数就会触发
    -  父向子传递数据，同时子组件触发点击事件
    -  现在，我们可以通过点击 Square 来填充那些方格，效果与之前相同。但是，当前 state 没有保存在单个的 Square 组件中，而是保存在了 Board 组件中。每当 Board 的 state 发生变化的时候，这些 Square 组件都会重新渲染一次。把所有 Square 的 state 保存在 Board 组件中可以让我们在将来判断出游戏的胜者。

    -  因为 Square 组件不再持有 state，因此每次它们被点击的时候，Square 组件就会从 Board 组件中接收值，并且通知 Board 组件。在 React 术语中，我们把目前的 Square 组件称做“受控组件”。在这种情况下，Board 组件完全控制了 Square 组件。注意，我们调用了 .slice() 方法创建了 squares 数组的一个副本，而不是直接在现有的数组上进行修改。在下一节，我们会介绍为什么我们需要创建 square 数组的副本。


        ```
        class Square extends React.Component {
        render() {
            return (
            <button
                className="square"
                onClick={() => this.props.onClick()}
            >
                {this.props.value}
            </button>
            );
        }
        }
        ```

        ```
        class Board extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
            squares: Array(9).fill(null),
            };
        }

        handleClick(i) {
            const squares = this.state.squares.slice();
            squares[i] = 'X';
            this.setState({squares: squares});
        }

        renderSquare(i) {
            return (
            <Square
                value={this.state.squares[i]}
                onClick={() => this.handleClick(i)}
            />
            );
        }

        render() {
            const status = 'Next player: X';

            return (
            <div>
                <div className="status">{status}</div>
                <div className="board-row">
                {this.renderSquare(0)}
                {this.renderSquare(1)}
                {this.renderSquare(2)}
                </div>
                <div className="board-row">
                {this.renderSquare(3)}
                {this.renderSquare(4)}
                {this.renderSquare(5)}
                </div>
                <div className="board-row">
                {this.renderSquare(6)}
                {this.renderSquare(7)}
                {this.renderSquare(8)}
                </div>
            </div>
            );
        }
        }
        ```

- 函数式组件
    - 如果你想写的组件只包含一个 render 方法，并且不包含 state，那么使用函数组件就会更简单。我们不需要定义一个继承于 React.Component 的类，我们可以定义一个函数，这个函数接收 props 作为参数，然后返回需要渲染的元素。函数组件写起来并不像 class 组件那么繁琐，很多组件都可以使用函数组件来写。
    ```
    function Square(props) {
    return (
        <button className="square" onClick={props.onClick}>
        {props.value}
        </button>
    );
    }
    ```

### react生命周期
- 挂载,当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下
    - constructor()
    - static getDerivedStateFromProps()
    - render()
    - componentDidMount()
- 更新,当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下
    - static getDerivedStateFromProps()
    - shouldComponentUpdate()
    - render()
    - getSnapshotBeforeUpdate()
    - componentDidUpdate()

- 卸载,当组件从 DOM 中移除时会调用如下方法：
   - componentWillUnmount()
- 错误处理,当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：
    - static getDerivedStateFromError()
    - componentDidCatch()
- 其他API
    - setState()
    - forceUpdate()

- class 属性
    - defaultProps：这一般用于 props 未赋值，但又不能为 null 的情况。
    ```
    class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
    ```
    - displayName
- 实例属性
    - props
    - state
### react 生命周期以及API讲解
- render
    - 如需与浏览器进行交互，请在 componentDidMount() 或其他生命周期方法中执行你的操作。保持 render() 为纯函数，可以使组件更容易思考。
- constructor()
    - 如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。
    - 通常，在 React 中，构造函数仅用于以下两种情况：通过给 this.state 赋值对象来初始化内部 state。为事件处理函数绑定实例

    ```
    constructor(props) {
  super(props);
  // 不要在这里调用 this.setState()
  this.state = { counter: 0 };
  this.handleClick = this.handleClick.bind(this);
    }
    ```
- componentDidMount()
    - componentDidMount() 会在组件挂载后（插入 DOM 树中）立即调用。依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。
- componentDidUpdate()
    - componentDidUpdate() 会在更新后会被立即调用。首次渲染不会执行此方法。当组件更新后，可以在此处对 DOM 进行操作。如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）。
        ```
        componentDidUpdate(prevProps) {
    // 典型用法（不要忘记比较 props）：
    if (this.props.userID !== prevProps.userID) {
        this.fetchData(this.props.userID);
    }
    }
        ```
- componentWillUnmount()
    - componentWillUnmount() 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅等。
    - componentWillUnmount() 中不应调用 setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。
- shouldComponentUpdate()
    - 根据 shouldComponentUpdate() 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。    
- static getDerivedStateFromProps()()
    - getDerivedStateFromProps 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。  
- getSnapshotBeforeUpdate()()
    - getSnapshotBeforeUpdate() 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 componentDidUpdate()。此用法并不常见，但它可能出现在 UI 处理中，如需要以特殊方式处理滚动位置的聊天线程等。应返回 snapshot 的值（或 null）。  

- 需特别注意，this.props.children 是一个特殊的 prop，通常由 JSX 表达式中的子组件组成，而非组件本身定义。

### 环境要求

React 16 依赖集合类型 Map 和 Set 。如果你要支持无法原生提供这些能力（例如 IE < 11）或实现不规范（例如 IE 11）的旧浏览器与设备，考虑在你的应用库中包含一个全局的 polyfill ，例如 core-js 或 babel-polyfill 。

### hook

- Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
