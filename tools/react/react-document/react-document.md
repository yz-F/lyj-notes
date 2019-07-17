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

