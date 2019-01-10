# vue中编写可复用的组件

## 1.vue中如何编写可复用的组件？？
>在编写组件的时候，时刻考虑组件是否可复用是有好处的。一次性组件跟其他组件紧密耦合没关系，但是可复用组件一定要定义一个清晰的公开接口。

Vue.js组件 API 来自 三部分：prop、事件、slot：

- prop 允许外部环境传递数据给组件，在vue-cli工程中也可以使用vuex等传递数据。
- 事件允许组件触发外部环境的 action
- slot 允许外部环境将内容插入到组件的视图结构内。

代码示例：

    <my-component
        :foo="bar"
        :bar="qux"
        //子组件调用父组件方法
        @event-a="doThis"
        @event-b="doThat">
        <!-- content -->
    <img slot="icon" src="..." />
    <p slot="main-text">Hello!</p>
    </my-component>

    