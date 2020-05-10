### 第三课vue基础第二天

1. 父子组件通信的几种方法

```
 第一种
 prop $emit
 第二种（父子组件层数过多）
 v-bind：$attrs v-on: $listener
 第三种
 中央事件总线（兄弟间的数据传递）
 新建一个vue事件Bus对象，然后通过bus.$emit触发事件，bus.$on监听触发事件
 第四种
 父组件通过provide提供变量，子组件通过inject注入变量，不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是只能从当前父组件中的prop属性来获取数据，只要在父组件的生命周期内，子组件都可以调用

```

### 第五节 vue-router-路由的使用
- 动态路由参数和查询参数
  ![111](../../image/b-ma/b-ma-08.png)
- 基本用法
  ![111](../../image/b-ma/b-ma-09.png)
- 命名路由
- 动态路由参数（需传id和params）
组件写法：参数从this.$params.id取出
  ![111](../../image/b-ma/b-ma-01.png)
路由写法：
  ![111](../../image/b-ma/b-ma-03.png)
全局路由匹配写法：
  ![111](../../image/b-ma/b-ma-04.png)
- 实例（例子中所显示的内容用的是动态路由匹配，而嵌套路由的用法用于所显示的内容不一致的情况）
掘金/首页/我的关注/热门
  ![111](../../image/b-ma/b-ma-02.png)

- 例子（嵌套路由路由匹配）
  - 组件内部写法（歌曲和内容的结构不一样，一个router-view里面嵌套另一个router-view）
  ![111](../../image/b-ma/b-ma-06.png)
  ![111](../../image/b-ma/b-ma-10.png)
  - 路由写法
    ![111](../../image/b-ma/b-ma-07.png)
- 动态路由匹配
  -  包括权限控制

### 第六节 axios-01  
- 动态路由匹配（公共组件的复用，通过watch监听$route对象，通过哦（params,id））
  - 路由在变，但是组件没有被重新渲染。
    ![111](../../image/b-ma/b-ma-11.png)
  - 文档
    ![111](../../image/b-ma/b-ma-13.png)
- 路由匹配模式 （默认是hash带#,history不带）
  ![111](../../image/b-ma/b-ma-14.png) 

- keep-alive用于路由里面保存当前点击状态，把组件状态缓存
  ![111](../../image/b-ma/b-ma-15.png) 
  ![111](../../image/b-ma/b-ma-16.png) 

- router中meta用于权限控制
  - redirect重定向
    ![111](../../image/b-ma/b-ma-18.png) 
  - 例子
    ![111](../../image/b-ma/b-ma-17.png) 
  ***4的用法是错误的，用的是路由守卫***
    ![111](../../image/b-ma/b-ma-19.png) 
    ![111](../../image/b-ma/b-ma-20.png) 
  - 路由守卫用法用于权限控制
    ![111](../../image/b-ma/b-ma-21.png) 
    ![111](../../image/b-ma/b-ma-22.png) 
    ![111](../../image/b-ma/b-ma-23.png) 

### 第七节 axios-02
- axios 例子
```
  this.$axios.get('url')
  .then(function(response){
    console.log(response)
  })
  .catch(function(error){
    console.log(error)
  })

  this.$axios.post('url',{
    firstName:'li',
    lastName:'yujia'
  })
  .then(function(response){
    console.log(response)
  })
  .catch(function(error){
    console.log(error)
  })
```
- axios执行多个并发请求
  ![111](../../image/b-ma/b-ma-24.png) 
  - 实例
  ![111](../../image/b-ma/b-ma-25.png) 
- axios对请求数据处理
  ![111](../../image/b-ma/b-ma-26.png)   


### 第八节 webpack的讲解
- axios请求拦截前，对请求头加token
  ![111](../../image/b-ma/b-ma-27.png) 

- 模拟用户登陆token操作（可以在加载数据前loading效果添加上去）
  ![111](../../image/b-ma/b-ma-28.png)   
  ![111](../../image/b-ma/b-ma-29.png)   

- 手动搭建webpack
```
npm init -- yes
npm install webpack@3.12.0 -D
```
- export的使用
  ![111](../../image/b-ma/b-ma-31.png)  
  ![111](../../image/b-ma/b-ma-34.png)  
  ![111](../../image/b-ma/b-ma-32.png)  
  ![111](../../image/b-ma/b-ma-33.png)  
- webpack脚本配置
  ![111](../../image/b-ma/b-ma-30.png)  

### 第九节 webpack的讲解

- 打包后的编译源码解析
```
0 // window对象
1 //
2 // vue源码本身
3 // 3，4，5都跟node_modules和setimmediate有关 Vue的异步更新机制有关
```
- webpack配置文件watch配置信息
  ![111](../../image/b-ma/b-ma-35.png)  

- 生产环境与开发环境不同webpack的配置
  ![111](../../image/b-ma/b-ma-36.png)  

- 相关loader处理
```
npm i css-loader style-loader -D
npm i url-loader file-loader -D

```
- img loader处理limit img大小 base64
  ![111](../../image/b-ma/b-ma-37.png) 
  ![111](../../image/b-ma/b-ma-38.png) 

- webpack热更新配置
  ![111](../../image/b-ma/b-ma-39.png) 


### 第十节 webpack+commonsChunc+ensure
  - npm install webpack-dev-serve --save-dev
  - 有关webpack-dev-serve的配置在package.json中
  ![111](../../image/b-ma/b-ma-40.png)
  - es6的处理 babel的配置
  ![111](../../image/b-ma/b-ma-41.png)
  - 单文件的引入
  ![111](../../image/b-ma/b-ma-42.png)
```
-D:辅助性的插件开发依赖(/package.json/devDependencis)
-S:当前项目依赖（/package.json/dependencies）

main.js下引入
import Vue from 'vue' //npm安装的vue
import Vue from './vue.js' //引入的包
```
- render函数挂载
  ![111](../../image/b-ma/b-ma-43.png)

### 第十一节 webpack+commonsChunc+ensure02
- CommonsChunkPlugin的使用
  ![111](../../image/b-ma/b-ma-44.png)
  ![111](../../image/b-ma/b-ma-45.png)

- CommonsChunkPlugin的配置
  ![111](../../image/b-ma/b-ma-46.png)
  ![111](../../image/b-ma/b-ma-47.png)

- require.ensure当你需要的时候去加载某个异步的文件
  ![111](../../image/b-ma/b-ma-48.png)
  ![111](../../image/b-ma/b-ma-49.png)
  ![111](../../image/b-ma/b-ma-50.png)
  ![111](../../image/b-ma/b-ma-51.png)

### vue-cli脚手架的使用
- 当给组件使用v-for遍历时，一定要加上:key属性，避免让vue帮咱们计算Dom
  ![111](../../image/b-ma/b-ma-52.png)

- 组件路由守卫
  ***当初始化路由时候，显示加载内容***
    ![111](../../image/b-ma/b-ma-53.png)
  ***当切换动态路由时候，路由组件内容不会跟着更新。此时就需要，watch监听路由变化并更新数据或者全局路由守卫beforeEach解决，或者路由组件beforeRouteUpdate监听路由变化***
   ![111](../../image/b-ma/b-ma-54.png)
  ***导航离开该组件时对应路由调用beforeRouterLeave,应用场景也可以是切换导航时候信息没填完提示继续填完用户信息。如果用户没有输入直接next()放行***
    ![111](../../image/b-ma/b-ma-55.png)
- restful规范
  - 命名(url/版本/表名)
    https://www.bootcss.com/v1/mycss
  - 尽量使用https，加密。http打包获取的数据是明文的  
  - 返回码
    ![111](../../image/b-ma/b-ma-57.png)

### vue项目第一天   

- 环境配置
  ```
  vue init webpack demo
  npm 安装mint ui
  main.js中配置引入全局组件并从node_module里面引入css样式
  npm install axios -S
  main.js中导入axios
  ```
- 导航路由匹配（this.$router.push编程式路由）
  ![111](../../image/b-ma/b-ma-58.png)
  ![111](../../image/b-ma/b-ma-59.png)
  ![111](../../image/b-ma/b-ma-60.png)
  ![111](../../image/b-ma/b-ma-61.png)
  ![111](../../image/b-ma/b-ma-62.png)

-   列表router-link跳转下一页面
![111](../../image/b-ma/b-ma-64.png)
![111](../../image/b-ma/b-ma-65.png)
- 除了上下导航栏，中间体内容布局样式滚动问题
![111](../../image/b-ma/b-ma-63.png)
- router-link返回问题
```
点击两下，返回上一级，再点击返回上一级 go(-1)

```

### vue项目第二天  
- [npm orm](https://www.npmjs.com/package/orm)

- src下和static下的区别
  ![111](../../image/b-ma/b-ma-66.png)

- 由于底部导航切换时候，状态显示按钮被点击
并且要求刷新后的状态也是被点击的状态
  ![111](../../image/b-ma/b-ma-67.png)
  ![111](../../image/b-ma/b-ma-68.png)
  ![111](../../image/b-ma/b-ma-69.png)

 -  底部导航所在路由对应的组件，点击进去之后，路由导航样式仍在。
![111](../../image/b-ma/b-ma-70.png)

- 全局组件封装
![111](../../image/b-ma/b-ma-71.png)
![111](../../image/b-ma/b-ma-72.png)
![111](../../image/b-ma/b-ma-73.png)

- momentjs
![111](../../image/b-ma/b-ma-74.png)
![111](../../image/b-ma/b-ma-75.png)
![111](../../image/b-ma/b-ma-76.png)

- 列表跳转列表详情页
![111](../../image/b-ma/b-ma-77.png)
![111](../../image/b-ma/b-ma-78.png)

- 懒加载
![111](../../image/b-ma/b-ma-79.png)
![111](../../image/b-ma/b-ma-80.png)

### vue项目第二天 02
- 当点击首页的时候，再从首页
某个链接点击进入到下一个页面的
时候，这个导航的选中仍存在
![111](../../image/b-ma/b-ma-81.png)
- 对路由进行点击@click需要加原生的标签 @click.native
![111](../../image/b-ma/b-ma-82.png)
- 导航守卫
![111](../../image/b-ma/b-ma-83.png)
![111](../../image/b-ma/b-ma-84.png)
![111](../../image/b-ma/b-ma-85.png)
![111](../../image/b-ma/b-ma-86.png)
![111](../../image/b-ma/b-ma-87.png)
![111](../../image/b-ma/b-ma-88.png)

### vue项目第三天 
![111](../../image/b-ma/b-ma-89.png)
1. 图片查看器 v-preview
    1. Vue.use(VuePreview) // 内部 vue.component('vue-preview',{})

2. 点击加载更多，每次点击数据增加一页
![111](../../image/b-ma/b-ma-90.png)
![111](../../image/b-ma/b-ma-91.png)

### vue项目第三天，上拉下拉刷新
1. 上拉下拉刷新？？？

2. 全局封装axios
![111](../../image/b-ma/b-ma-94.png)
![111](../../image/b-ma/b-ma-95.png)

3. 路由跳转标题
![111](../../image/b-ma/b-ma-96.png)
![111](../../image/b-ma/b-ma-97.png)

4. moment全局时间封装
![111](../../image/b-ma/b-ma-98.png)

5. 动态路由
![111](../../image/b-ma/b-ma-99.png)

### vue项目第五天 - 购物车实现 -01

### vue项目第五天 - 购物车实现 -02

