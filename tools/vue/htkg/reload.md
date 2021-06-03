- 控制面板调用全局方法
```

template>
  <div id="app"
       style="height:100%">
    <router-view ref="appRouterView"></router-view>
  </div>
</template>

<script>

export default {
  name: 'app',
  provide () {
    return {
      reload: this.reload
    }
  },
  methods: {

    // 解决登录以及切换企业后重新加载菜单以及用户资源方法
    reload (open) {
      this.$nextTick(() => {
        this.$refs.appRouterView.layoutLifeCommonFunc()
      })
    },
```
- inject
```
  inject: ['reload'],
  methord(){
    this.reload()
  }
```