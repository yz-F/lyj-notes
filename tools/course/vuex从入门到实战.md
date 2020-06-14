### state三种引入方式
```
1. $store.state.aaa

2. computed:{
  ...mapState([aaa])
}
import mapstate from vuex
```

### mutation

```
1. 触发 this.$store.commit("add")
2. 触发 调方法式触发

import (mapMutations) from 'vuex'

methods:{
  ...mapMutations(['add','del'])
}
3.mutation不能写异步操作
```

### mutation 参数

```
1. 触发 this.$store.commit("add",3)
```
```
mutation:{
  add(state, params){
    state.num = state.num+params
  }
}
```
### action异步操作，action通过mutation方式变更数据

```

this.$store.dispatch('asyncadd',params)
```

```
import {mapActions} from vuex
...mapAction(['ayncadd'])
```
### getter
```
1.this.$store.getter.getadd

2.import {mapGetters} from 'vuex'
computer:{
  ...mapGetters(['getadd'])
}
3.getter相当与计算属性的作用
```

### vuex todo
![111](../../image/v-vuex/vuex-00.png)
1. 将数据与vuex绑定
![111](../../image/v-vuex/vuex-01.png)


3. 实现文本框内容的双向同步
![111](../../image/v-vuex/vuex-02.png)
state添加inputValue,组件绑定:value = inputValue ,添加change事件触发mutation,实现文本框内容的双向同步

4. 向列表添加一项
![111](../../image/v-vuex/vuex-03.png)
```
1.添加的数据不能为空
2.添加一项时候，自动赋给其状态,done:false为默认状态，没有勾选
```


5. 删除某一项,根据id.mutation来删除数组的某一项
![111](../../image/v-vuex/vuex-04.png)

6. 复选框的状态绑定数组的状态
![111](../../image/v-vuex/vuex-05.png)

7. 点击复选框获取状态以及id
![111](../../image/v-vuex/vuex-06.png)
![111](../../image/v-vuex/vuex-07.png)
![111](../../image/v-vuex/vuex-08.png)

8. 修改列表项的选中状态，赋予复选框新的选中状态
![111](../../image/v-vuex/vuex-09.png)

9. 统计未完成的状态，getters
![111](../../image/v-vuex/vuex-10.png)

10. 清除已完成的
![111](../../image/v-vuex/vuex-11.png)

11. 全部，未完成，已完成
点击不同的按钮，修改vuex对应的viewkey值
type = primary，按钮高亮
![111](../../image/v-vuex/vuex-12.png)
![111](../../image/v-vuex/vuex-13.png)
![111](../../image/v-vuex/vuex-14.png)

12. geteers修改数据源
![111](../../image/v-vuex/vuex-15.png)
