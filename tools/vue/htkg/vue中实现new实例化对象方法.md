### src\modules\app\domain\App.js（class封装，注意vm，可以从vue页面传入当前组件实列，相当于调用this）

- app.js

```
class AppCenterSoftware {
  constructor(params) {
    //应用相关的属性
    this.apptype = params.apptype
  } 
  //获取授权后 安装或者打开应用

  // vm 当前的组件实例, content 后后返回的授权信息。

  async installAndOpen (vm, content) {
    console.log(this.getMyInfoState(), '是否在我的应用中')
    if (this.getMyInfoState()) {
      // 是否储存在本地应用中。
      if (this.apptype === '0') {
        // // 是否已安装
        if (vm.isLoginState && this.loadInstalledAppsList(vm)) {
          // 是否储存了相关信息
          if (this.getAppInfoState()) {
            // 是否存储相关软件信息（直接复制过来，可能没存取相关信息）
            this.openApp(content) // 直接打开应用
          } else {
            // 未储存软件信息，在appinfos进行储存
            await this.saveAppInfo(vm)
            this.openApp(content) // 直接打开应用
          }
        } else {
          this.down(vm)
        }
      } else if (this.apptype === '1') {
        // 应用为网页
        console.log('进入网页')
        console.log(this.getAppInfoState(), "是否缓存")
        //判断是否本地缓存了相关信息
        if (!this.getAppInfoState()) {
          //未缓存  先缓存  再打开
          this.downloadTransition(vm)
        }
        this.openWebApp(vm, content)
      } else if (this.apptype === '2') {
        console.log("进入判断环节")
        // 应用为网页(单点登录)
        if (!this.getAppInfoState()) {
          // 是否存储相关软件信息（直接复制过来，可能没存取相关信息）
          this.downloadTransition(vm)
          this.getUrl(vm) // 直接打开应用
          return
        }
        this.getUrl(vm) // 直接打开应用
      }
    } else {
      console.log('不在我的应用中')
      // 不在我的应用中
      if (this.apptype === '0') {
        // 为软件
        this.addToMyAppStore(vm)
        vm.loadReSource()
        // 是否已安装，不需要下载程序
        if (!(vm.isLoginState && this.loadInstalledAppsList(vm))) {
          // 执行下载操作并且添加到我的应用中，需要调接口（添加我的应用的接口),并且存在appinfos中
          this.down(vm)
        }
      } else if (this.apptype === '1' || this.apptype === '2') {
        // 为网页直接打开
        await this.openWebApp(vm, content, true)
      }
    }
  }

  export default AppCenterSoftware
```

- appCenter.js中引用 Appjs封装的类（并将当前组件实列传给app.js）

```

import AppCenterSoftware from '../modules/app/domain/App'

// item 就是类的实例
 let searchData = params.length > 0 ? params : []
        searchData.map((item, index, arr) => {
          item = new AppCenterSoftware(item)
          return (arr[index] = item)
        })
        this.appArray = searchData
        // 成功后，跳转到我的应用模块，进行赋值
        // this.activeName = 'allApp'
        this.$nextTick().then((this.tempAppInfoList = this.allAppArray))

// item就可以调用类的实例方法

```