> 常用的两种刷新方法

```
window.location.reload()，原生 js 提供的方法；

this.$router.go(0)，vue 路由里面的一种方法；

```

> 无痕刷新

- 先在全局组件注册一个方法，用该方法控制router-view的显示与否，然后在子组件调用。
  1. v-if控制```<router-view></router-view>```的显示
  2. provide：全局注册方法。
  3. methods：设置reload方法

  ```

    <div id="app">
            <router-view v-if="isRouterAlive"></router-view>
    </div>



    provide(){
        return{
                reload:this.reload
        }
    },
    data(){
        return {
                isRouterAlive:true
        }
    },
    methods:{
        reload(){
            this.isRouterAlive = false;
            this.$nextTick(function(){
                this.isRouterAlive = true;
            })
        }
    }
}
  ```

  - 使用

  ```


    inject:['reload'],
    mounted(){
        this.reload();
    }

  ```