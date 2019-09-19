### 点击当前页面商品，路由跳转参数Id变化所对应的的详情页
- 监听路由变化，并且改变跳转新的Id参数。重新调详情页接口
```
    watch: {
    '$route' (to, from) {
      debugger
      var id = this.$route.params.pId
      this.$router.push({
        path: '/co-pr/detail/' + id
      })
      this.$emit('refreshCurrentPage', id)
    }
  },
```

- 重新调详情页接口

```
 refreshCurrentPage (param) {
    debugger
    this.getGoodsDetailDataApi({productId: Number(param)})
},

```