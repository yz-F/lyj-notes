### mixin除了可以混入方法

### minxin也可以混入页面

- src\mixins\appCenter.js(页面结构与vue类似但又不同,定义loadReSource)
```
import $store from '../store'

export default {
  name: '',
  components: {
    // 调用组件
    messageSubmit,

  },
  data () {},
  computed: {},
  // 这里面的方法需要页面调用才能调用
  methods: {
    // 请求我的应用的数据
    async loadUserApp () {
      await getUserAppInfo().then((res) => {
        if (res.code === '000') {
          this.userAppArray = res.result
          this.userAppArray.forEach(
            (item, index, arr) => {
              item = new AppCenterSoftware(item)
              return arr[index] = item
            }
          )
          this.tempAppInfoList = this.userAppArray
          let store = new Store()
          store.set('myAppinfo', this.userAppArray)
        } else {
          if (res.code === '356') {
            this.userAppArray = []
            if (this.activeName === 'userApp') {
              this.showNoData = false
            }
            let store = new Store()
            store.set('myAppinfo', this.userAppArray)
          } else {
            this.$message.error(res.msg)
          }
        }
      })
    },
    // .vue页面直接混入调用
      handleClick (data) {
        // 此处也可以控制别的页面的数据
      this.$parent.$parent.$parent.searchContent = ''
      if (this.activeName === 'allApp') {
        this.appArray = this.allAppArray
        this.tempAppInfoList = this.allAppArray
        this.$forceUpdate()
      } else {
        this.appArray = this.allAppArray.filter(
          (item) => item.classify === data.name
        )
        this.tempAppInfoList = this.appArray
        this.$forceUpdate()
      }
    },
    // 加载数据资源，初始化部分变量
    async loadReSource () {}，
     //游客模式按钮点击事件
    toLogin (item) {
      this.$CommonFunction.checkLoginState(this.$refs.login, item.qxbz)
    },

  },
  // 这里面的方法会mixin就调用
  async mounted () {},
  beforeDestroy () {
    if (this.unSubscriberProgressLis) {
      this.unSubscriberProgressLis()
    }
    console.log('应用市场及详情页面清除订阅')
  },

}  

```

- vue页面混入mixin页面(loadReSource)

```
import appCenter from '../../../../mixins/appCenter'

export default {
  name: '',
  data() {
    return {
      activeName: 'userApp',
    }
  },
  mixins: [appCenter],
  mounted() {
    if (localStorage.getItem('access_token')) {
      this.loadReSource()
    } else {
      this.userAppArray = []
      this.showNoData = false
    }
  },

}  
```

- minxin.js (appCenter.js里面的this,可以控制vue的当前实例)（注意这里的this,this.$parent.$parent.$parent.searchContent）

```
 // 按钮点击事件
    clickButton: debounce(async function (item, buttonType) {
      this.$CommonFunction.checkDir()
      console.log(item, '下载应用信息')
      if (this.$router.history.current.name === 'productDetail') {
        if (localStorage.getItem('access_token')) {
          await this.loadReSource()
        }
        this.appItem = this.appArray.filter(
          (item) => item.proid === this.appItem[0].proid
        )
        this.$forceUpdate()
      } else {
        if (this.$parent.$parent.$parent.searchContent) {
          this.searchApp(this.$parent.$parent.$parent.searchContent)
        } else {
          if (localStorage.getItem('access_token')) {
            await this.loadReSource()
          }
          //刷新相关按钮
        }
      }

```
