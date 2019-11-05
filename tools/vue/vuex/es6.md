### 解构赋值

```
 //upload模块 撤销预览
if(type === 'upload-pending'){

    const {id} = {row} //解构参数，row为对象，取出id值
    this.getApprovalListApi({id, status:3});//添加参数
}
```

### async await

> 此种方法，一次请求传入数据,await后面接一个promise，但是async将方法getTopTradeListDataApi()封装成一个promise。所以组件之中可以再次await

- vuex

```
    async getTopTradeListDataApi ({ state, commit }) {
        let { data: res } = await getTopTradeListData({...state.topTradeParams})
        commit(UPDATE_TOPTRADEDATA, res)
    },
```
- component

```
methords : {
    async getTopTradeList () {
      await this.getTopTradeListDataApi() //组件之中可以再次await
      this.trdLstBeans = this.topTradeData.records
    },

    mounted () {
        this.getTopTradeList()
    }
}    

```

> 此种写法，需要点击请求两次才能传入数据,this.getSwiperListDataApi();是异步回调，所以this.hotConcepts = this.swiperListData.records执行完后才执行this.getSwiperListDataApi();所以为了让他们按顺序执行，则需要封装一层async。让其在里面按顺序执行。

- vuex

```
     async getSwiperListDataApi ({ state, commit }) {
        // const { tableSource, size, current, desc } = { ...params }
        let { data: res } = await getSwiperListData({...state.params, positionGroup: 'New'})
        // console.log(res.data);
        commit(UPDATE_SWIPERLISTDATA, res)
        // commit(UPDATE_TOTAL, res.total)
    },
```
- component
```
     pageInit () {
      this.getSwiperListDataApi();
      this.hotConcepts = this.swiperListData.records
    },
```

### 箭头函数中

- 没有this,this是外部的函数的。