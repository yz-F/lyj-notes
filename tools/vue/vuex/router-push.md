### 点击跳转
```
    <div @click="goDetail(item.id)" class="concept-img-wrapper">
        <img :src="item.mainImgSrc" :alt="item.title">
    </div>
```
// 带查询参数，变成/co-co/euperlan?cptId=2
```
 goDetail (id) {
      // console.log('id',id)
      this.$router.push({
        path: '/co-co/euperlan',
        query: {
          cptId: id
        }
      })
    },
```

### 使用参数
```
mounted () {
    // this.newConceptDetailAction({id: this.$route.query.cptId})
    this.getDetailListDataApi({id: this.$route.query.cptId})
    console.log(this.getDetailListData)
    debugger
    this.datas = this.getDetailListData
  },

```
