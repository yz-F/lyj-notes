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
![111](../../../image/b-ma/b-ma-08.png)
- 基本用法
![111](../../../image/b-ma/b-ma-09.png)
- 命名路由
- 动态路由参数（需传id和params）
组件写法：参数从this.$params.id取出
![111](../../../image/b-ma/b-ma-01.png)
路由写法：
![111](../../../image/b-ma/b-ma-03.png)
全局路由匹配写法：
![111](../../../image/b-ma/b-ma-04.png)
- 实例（例子中所显示的内容用的是动态路由匹配，而嵌套路由的用法用于所显示的内容不一致的情况）
掘金/首页/我的关注/热门
![111](../../../image/b-ma/b-ma-02.png)

- 例子（嵌套路由路由匹配）
  - 组件内部写法（歌曲和内容的结构不一样，一个router-view里面嵌套另一个router-view）
![111](../../../image/b-ma/b-ma-06.png)
![111](../../../image/b-ma/b-ma-10.png)
  - 路由写法
  ![111](../../../image/b-ma/b-ma-07.png)
- 动态路由匹配
  -  包括权限控制

### 第六节 axios-01  
- 动态路由匹配（公共组件的复用，通过watch监听$route对象，通过哦（params,id））
  - 路由在变，但是组件没有被重新渲染。
  ![111](../../../image/b-ma/b-ma-11.png)
  - 文档
  ![111](../../../image/b-ma/b-ma-13.png)
- 路由匹配模式 （默认是hash带#,history不带）
![111](../../../image/b-ma/b-ma-14.png) 

- keep-alive用于路由里面保存当前点击状态，把组件状态缓存
![111](../../../image/b-ma/b-ma-15.png) 
![111](../../../image/b-ma/b-ma-16.png) 

- router中meta用于权限控制
  - redirect重定向
  ![111](../../../image/b-ma/b-ma-18.png) 
  - 例子
  ![111](../../../image/b-ma/b-ma-17.png) 
  ***4的用法是错误的，用的是路由守卫***
  ![111](../../../image/b-ma/b-ma-19.png) 
  ![111](../../../image/b-ma/b-ma-20.png) 
  - 路由守卫用法用于权限控制
  ![111](../../../image/b-ma/b-ma-21.png) 
  ![111](../../../image/b-ma/b-ma-22.png) 
  ![111](../../../image/b-ma/b-ma-23.png) 

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
![111](../../../image/b-ma/b-ma-24.png) 
  - 实例
  ![111](../../../image/b-ma/b-ma-25.png) 
- axios对请求数据处理
  ![111](../../../image/b-ma/b-ma-26.png)   


### 第八节 webpack的讲解
- axios请求拦截前，对请求头加token
  ![111](../../../image/b-ma/b-ma-27.png) 

- 模拟用户登陆token操作（可以在加载数据前loading效果添加上去）
  ![111](../../../image/b-ma/b-ma-28.png)   
  ![111](../../../image/b-ma/b-ma-29.png)   

- 手动搭建webpack
```
npm init -- yes
npm install webpack@3.12.0 -D
```
- export的使用
![111](../../../image/b-ma/b-ma-31.png)  
![111](../../../image/b-ma/b-ma-34.png)  
![111](../../../image/b-ma/b-ma-32.png)  
![111](../../../image/b-ma/b-ma-33.png)  
- webpack脚本配置
![111](../../../image/b-ma/b-ma-30.png)  

### 第九节 webpack的讲解

- 打包后的编译源码解析
```
0 // window对象
1 //
2 // vue源码本身
3 // 3，4，5都跟node_modules和setimmediate有关 Vue的异步更新机制有关
```
- webpack配置文件watch配置信息
![111](../../../image/b-ma/b-ma-35.png)  

- 生产环境与开发环境不同webpack的配置
![111](../../../image/b-ma/b-ma-36.png)  

- 相关loader处理
```
npm i css-loader style-loader -D
npm i url-loader file-loader -D

```
- img loader处理limit img大小 base64
![111](../../../image/b-ma/b-ma-37.png) 
![111](../../../image/b-ma/b-ma-38.png) 

- webpack热更新配置
![111](../../../image/b-ma/b-ma-39.png) 


### 第十节 webpack+commonsChunc+ensure
- npm install webpack-dev-serve --save-dev
- 有关webpack-dev-serve的配置在package.json中
![111](../../../image/b-ma/b-ma-40.png)
- es6的处理 babel的配置
![111](../../../image/b-ma/b-ma-41.png)
- 单文件的引入
![111](../../../image/b-ma/b-ma-42.png)
```
-D:辅助性的插件开发依赖(/package.json/devDependencis)
-S:当前项目依赖（/package.json/dependencies）

main.js下引入
import Vue from 'vue' //npm安装的vue
import Vue from './vue.js' //引入的包
```
- render函数挂载
![111](../../../image/b-ma/b-ma-43.png)

### 第十一节 webpack+commonsChunc+ensure02
- CommonsChunkPlugin的使用
![111](../../../image/b-ma/b-ma-44.png)
![111](../../../image/b-ma/b-ma-45.png)

- CommonsChunkPlugin的配置
![111](../../../image/b-ma/b-ma-46.png)
![111](../../../image/b-ma/b-ma-47.png)

- require.ensure当你需要的时候去加载某个异步的文件
![111](../../../image/b-ma/b-ma-48.png)
![111](../../../image/b-ma/b-ma-49.png)
![111](../../../image/b-ma/b-ma-50.png)
![111](../../../image/b-ma/b-ma-51.png)

### vue-cli脚手架的使用
- 当给组件使用v-for遍历时，一定要加上:key属性，避免让vue帮咱们计算Dom