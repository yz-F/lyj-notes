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
- router-link跳转写法

```
<router-link tag="li" :to="'/co-pr/detail/' + item.id" class="zdh-pointer" v-for="(item,idx) in products" :key="idx">
    <div class="pro-img-wrapper">
        <img class="zdh-pro-item-img" :src="item.image" alt="">
    </div>
    <span class="pro-name">{{item.title}}</span>
    <span class="pro-effect">{{item.subCategory}}</span>
    <span class="pro-price"><span>{{$t('txt.prodProps.price')}} </span>{{item.sellPrice}}</span>
</router-link>
```

- 重新调详情页接口

```
 refreshCurrentPage (param) {
    debugger
    this.getGoodsDetailDataApi({productId: Number(param)})
},

```