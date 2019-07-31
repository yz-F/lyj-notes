### computed
![eolink](../../../image/vue/vuex/vuex3.png)
![eolink](../../../image/vue/vuex/vuex4.png)

> 不用在data里面定义，根据isUpload返回需要的数组

```
computed:{
    ...mapGetters('dictionary',['regionList']),
    ...mapGetters("concept", ["tableData","pager","params"]),
    tabOption(){
      const isUpload = !this.isUpload
      const list = [
        {
          label: 'All',
          value: '1',
          isShow: isUpload
        },
        {
          label: 'Approved',
          value: '2',
          isShow: true
        },
        {
          label: 'Pending Approval',
          value: '3',
          badge: '100+',
          isShow: true
        },
        {
          label: 'Denied',
          value: '4',
          badge: '10',
          isShow: isUpload
        },
        {
          label: 'Draft',
          value: '5',
          isShow: true
        }
      ]
      return {
        activeName: '1',
        list: list.filter(v => v.isShow)
      }
    }
  },

```