### webview形式打开网页

```
<webview :src="url"
  class="web"
  style="height:calc(100% - 30px);"
  id="qfsc"></webview>

```

```
const webview = document.getElementById('qfsc')
webview.addEventListener('dom-ready', () => {
  this.showBtnStatus()
  this.initLoading = false
})

webview.addEventListener('new-window', (e) => {
  const protocol = require('url').parse(e.url).protocol
  if (protocol === 'http:' || protocol === 'https:') {
    webview.src = e.url
  }
})

```

```
<el-button icon="el-icon-arrow-left"
  title="后退"
  style="border:none;padding:8px 15px;background-color: transparent;"
  :disabled="gobackDisable"
  @click="goBackWebView()">
</el-button>
<el-button icon="el-icon-arrow-right"
  title="前进"
  style="border:none;padding:8px 15px;background-color: transparent;"
  :disabled="goforwardDisable"
  @click="goForwardWebView()">
</el-button>

 showBtnStatus () {
  this.gobackDisable = !document.getElementById('qfsc').canGoBack()
  this.goforwardDisable = !document.getElementById('qfsc').canGoForward()
},
```
### electron内置浏览器形式打开网页

```
// 打开系统浏览器
openSystemBrowser(url) {
  let shell = require('electron').shell
  shell.openExternal(url)
},

// 

this.$CommonFunction.openSystemBrowser(item.weburl + '?token=' + content.result)
```

### todo.js

- 除了把逻辑放在vuex里面,也可以把逻辑放在todo.js 文件里面

```
import {getAppConfig} from './getallConfig'
import platformApi from '../api/platform'
import deviceService from './device'
import moment from 'moment'
import {getServiceChargeApi, getPanItemApi} from '../api/setting.js'
import $store from '../store'
import AES from '../assets/js/AES.js'

/**
 * 设备服务
 */
export default {
  // 发票库存报警值
  invoiceStorageAlertNum: 501,
  // 是否有财税服务权限
  hasCsfw() {
    let csfw = getAppConfig('axn_csfw')
    console.log('csfw', csfw)
    if (csfw && csfw.xmlbVos) {
      return csfw && csfw.xmlbVos.length > 0
    } else {
      return false
    }
  },
  async queryToDoList(userType, todoList) {
    console.log('开始获取待办事项...')
    let todo = {}
    // let todo = {
    //   info: '', // 事项描述
    //   dt: '', // 事项日期
    //   type: '', // 事项类型 （serviceCharge）服务费  （上报汇总，清卡，发票申请）51事项 （taxclound）单机事项
    //   source: '' // 数据来源 platform 后台  taxclound 中台
    // }
    let list = []
    // 2. 如果是企业用户，
    
    if (userType === 1) {
      // 是否有财税服务权限
      console.log("this.$store.state.auth.menuOptions",$store.state.auth.menuOptions)
      const hasCsfw = $store.state.auth.menuOptions.findIndex((item) => {
        // return item.qxdm === 'axn_csfw' && item.status === 1
        return item.qxdm === 'axn_csfw'
      })
      console.log("是否有财税服务权限",hasCsfw)
      if (hasCsfw == -1) {
        console.log('没有财税服务权限')
        let info = JSON.parse(localStorage.getItem('info'))
        let data = {token: info.token, nsrsbh: info.taxcode}
        try {
          var serviceChargeTwo = await getServiceChargeApi(data)
          if (serviceChargeTwo && serviceChargeTwo.code === '000') {
            var serviceChargeTwoRes = JSON.parse(AES.decrypt(serviceChargeTwo.result))
            if (serviceChargeTwoRes && serviceChargeTwoRes.fwfzt === 0) {
              // 1-未到期  0需要提示
              todo = {
                info: '服务费到期-缴款', // 事项描述
                dt: moment().format('MM-DD'), // 事项日期
                type: 'serviceCharge', // 事项类型
                source: 'platform',
                url:serviceChargeTwoRes.jfUrl ? serviceChargeTwoRes.jfUrl:''
              }
              list.push(todo)
            }
            return list
          }
        } catch (error) {
        }
      } else {
        // 有财税服务权限，判断盘是否在线
        console.log('有财税服务权限,开始检测插入设备状态')
        let device = deviceService.detect()
        console.log(device, 'device')
        let infoJson = JSON.parse(localStorage.getItem('info'))
        // 3. 判断是否插盘、
        if (device.isDevice) {
          console.log('已插入设备:', device)
          // 4. 是否与当前所属企业一致
          // todo 目前不支持Ukey
          if (device.info.type === 'jsp' && infoJson.taxcode !== device.info.taxNumber) {
            // 盘与当前企业不一致结束流程
            console.log('插入从设备与当前企业不一致')
            return todoList
          } else {
            console.log('插入从设备与当前企业一致')
            // 调用中台
            if (device.info.type === 'jsp') {
              // 1. 调用中台客户端获取 领购、抄税、清卡 调用后台获取 服务费缴纳
              let taxCloundList = await this.getTaxClound()
              list = list.concat(taxCloundList)
            }
            //
            // let info = JSON.parse(localStorage.getItem('info'))
            let datares = {token: infoJson.token, nsrsbh: infoJson.taxcode}
            try {
              let serviceChargeThree = await getServiceChargeApi(datares)
              if (serviceChargeThree && serviceChargeThree.code === '000') {
                var serviceChargeThreeRes = JSON.parse(AES.decrypt(serviceChargeThree.result))
                console.log("serviceChargeThreeRes",serviceChargeThreeRes)
                if (serviceChargeThreeRes && serviceChargeThreeRes.fwfzt === 0) {
                  // 1-未到期  0需要提示
                  todo = {
                    info: '服务费到期-缴款', // 事项描述
                    dt: moment().format('MM-DD'), // 事项日期
                    type: 'serviceCharge', // 事项类型
                    source: 'platform',
                    url: serviceChargeThreeRes.jfUrl ? serviceChargeThreeRes.jfUrl:''
                  }
                  list.push(todo)
                }
              }
            } catch (error) {
              console.log(error)
            }
          }
        } else {
          console.log('未插入设备:')
          // debugger
          // 5. 调用后台获取 领购、抄税、清卡、服务费缴纳
          // 调用服务费缴纳接口 调用税盘事项接口，
          // let info = JSON.parse(localStorage.getItem('info'))
          let dataRes = {token: infoJson.token, nsrsbh: infoJson.taxcode}
          try {
            var serviceChargeOne = await getServiceChargeApi(dataRes)
            if (serviceChargeOne && serviceChargeOne.code === '000') {
              var serviceChargeOneRes = JSON.parse(AES.decrypt(serviceChargeOne.result))
              if (serviceChargeOneRes && serviceChargeOneRes.fwfzt === 0) {
                // 1-未到期  0需要提示
                todo = {
                  info: '服务费到期-缴款', // 事项描述
                  dt: moment().format('MM-DD'), // 事项日期
                  type: 'serviceCharge', // 事项类型
                  source: 'platform',
                  url:serviceChargeOneRes.jfUrl ? serviceChargeOneRes.jfUrl:''
                }
                list.push(todo)
              }
            }
          } catch (error) {

          }
          let infos = JSON.parse(localStorage.getItem('info'))
          let datas = {token: infos.token, nsrsbh: infos.taxcode}
          try {
            var panItem = await getPanItemApi(datas)
            if (panItem && panItem.code === '000') {
              var panItemRes = JSON.parse(AES.decrypt(panItem.result))
              console.log("panItemRes",panItemRes)
              if (panItemRes && panItemRes.cbzt === 1) {
                todo = {
                  info: '抄报清卡状态-上报汇总', // 事项描述
                  dt: moment().format('MM-DD'), // 事项日期
                  detail: '每月征期内需完成开票软件上报汇总工作，进入开票软件会自动完成，建议每月初启动开票软件一次完成此工作。',
                  type: '上报汇总', // 事项类型
                  source: 'platform',
                  url:''
                }
                list.push(todo)
              } else if (panItemRes && panItemRes.cbzt === 2) {
                todo = {
                  info: '抄报清卡状态-清卡', // 事项描述
                  dt: moment().format('MM-DD'), // 事项日期
                  detail: '每月征期内需完成开票软件清卡工作，在上报汇总和申报完成后进入开票软件系统会自动完成(票表对比相符)。',
                  type: '清卡', // 事项类型
                  source: 'platform',
                  url:''
                }
                list.push(todo)
              }
              if (panItemRes.fpkcList) {
                let details = []
                let obj = {}
                for(let i = 0;i<panItemRes.fpkcList.length;i++){
                  obj = {fpzl: panItemRes.fpkcList[i].fpzlMc, zsyfs: panItemRes.fpkcList[i].zsyfs}
                  details.push(obj)
                }
                // for (let key in panItemRes.fpkcList) {
                //   console.log(key + '---')
                //   details.push({fpzl: panItemRes.fpkcList[key].fpzlMc, zsyfs: panItemRes.fpkcList[key].zsyfs})
                // }
                todo = {
                  info: '发票库存不足-申领', // 事项描述
                  dt: moment().format('MM-DD'), // 事项日期
                  detail: details,
                  type: '发票申领', // 事项类型
                  source: 'platform',
                  url:''
                }
                list.push(todo)
              }
            }
          } catch (error) {

          }
        }
      }
    } else if (userType === 0) {
      // // 下期游客如果插盘需要调用中台接口
      // let device = deviceService.detect()
      // // 判断是否插盘、
      // if (device.isDevice) {
      //   console.log('插入从设备与当前企业一致')
      //   // 调用中台
      //   // 1. 调用中台客户端获取 领购、抄税、清卡 调用后台获取 服务费缴纳
      //   let taxCloundList = this.getTaxClound()
      //   list = list.concat(taxCloundList)
      // }
      return []
    } else {
      console.log('非企业用户，不显示待办事项')
      list = []
    }
    return list
  },
  //
  async getTaxClound() {
    let todo = {}
    let list = []
    let token = await platformApi.getToken()
    let deviceStateResult = await platformApi.getDeviceState('local', token)
    console.log('getDeviceState:', deviceStateResult)

    let invoiceStorageResult = await platformApi.getInvoiceStorage('local', token)
    console.log('getInvoiceStorage:', invoiceStorageResult)
    if (deviceStateResult.code === '000000') {
      // 抄报清卡状态-上报汇总  1-未抄税，2-抄税未清卡，3-已清卡，4-已锁死
      // 只要有一个票种的状态出现1，则生成事项“抄报清卡状态-上报汇总”
      if ((deviceStateResult.datagram.dzfp.cbqkzt === '1' && deviceStateResult.datagram.dzfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.jdcxstyfp.cbqkzt === '1' && deviceStateResult.datagram.jdcxstyfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.jsfp.cbqkzt === '1' && deviceStateResult.datagram.jsfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.zzsptfp.cbqkzt === '1' && deviceStateResult.datagram.zzsptfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.zzszyfp.cbqkzt === '1' && deviceStateResult.datagram.zzszyfp.lxxe !== '-1.00')) {
        todo = {
          info: '抄报清卡状态-上报汇总', // 事项描述
          dt: moment().format('MM-DD'), // 事项日期
          detail: '每月征期内需完成开票软件上报汇总工作，进入开票软件会自动完成，建议每月初启动开票软件一次完成此工作。',
          type: '上报汇总', // 事项类型
          source: 'taxclound',
          url:''
        }
        list.push(todo)
      }
      // 抄报清卡状态-清卡
      // 只要有一个票种的状态出现2，则生成事项“抄报清卡状态-清卡”
      if ((deviceStateResult.datagram.dzfp.cbqkzt === '2' && deviceStateResult.datagram.dzfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.jdcxstyfp.cbqkzt === '2' && deviceStateResult.datagram.jdcxstyfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.jsfp.cbqkzt === '2' && deviceStateResult.datagram.jsfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.zzsptfp.cbqkzt === '2' && deviceStateResult.datagram.zzsptfp.lxxe !== '-1.00') ||
          (deviceStateResult.datagram.zzszyfp.cbqkzt === '2' && deviceStateResult.datagram.zzszyfp.lxxe !== '-1.00')) {
        todo = {
          info: '抄报清卡状态-清卡', // 事项描述
          dt: moment().format('MM-DD'), // 事项日期
          detail: '每月征期内需完成开票软件清卡工作，在上报汇总和申报完成后进入开票软件系统会自动完成(票表对比相符)。',
          type: '清卡', // 事项类型
          source: 'taxclound',
          url:''
        }
        list.push(todo)
      }
    }
    // 任意一个票种发票总剩余份数<=预警值时，生成事项“发票库存-申领”
    if (invoiceStorageResult.code === '000000') {
      let storageDetail = []
      for (let i = 0; i < invoiceStorageResult.datagram.length; i++) {
        if (parseInt(invoiceStorageResult.datagram[i].zsyfs) < this.invoiceStorageAlertNum &&
            this.getStorageEnabledByLxxe(invoiceStorageResult.datagram[i].fpzl_dm, deviceStateResult)) {
          storageDetail.push({
            fpzl: this.getFpzlName(invoiceStorageResult.datagram[i].fpzl_dm),
            zsyfs: invoiceStorageResult.datagram[i].zsyfs
          })
        }
      }
      if (storageDetail.length > 0) {
        todo = {
          info: '发票库存不足-申领', // 事项描述
          dt: moment().format('MM-DD'), // 事项日期
          detail: storageDetail,
          type: '发票申领', // 事项类型
          source: 'taxclound',
          url:''
        }
        list.push(todo)
      }
    }
    return list
  },
  // 根据发票库存票种，从税控设备状态表中获取对应票种的离线限额
  getStorageEnabledByLxxe(fpzldm, deviceStateResult) {
    if (fpzldm === '0') {
      return deviceStateResult.datagram.zzszyfp.lxxe !== '-1.00'
    } else if (fpzldm === '2') {
      return deviceStateResult.datagram.zzsptfp.lxxe !== '-1.00'
    } else if (fpzldm === '41') {
      return deviceStateResult.datagram.jsfp.lxxe !== '-1.00'
    } else if (fpzldm === '51') {
      return deviceStateResult.datagram.dzfp.lxxe !== '-1.00'
    } else if (fpzldm === '12') {
      return deviceStateResult.datagram.jdcxstyfp.lxxe !== '-1.00'
    }
    return true
  },
  getFpzlName(fpzldm) {
    if (fpzldm === '0') {
      return '增值税专用发票'
    } else if (fpzldm === '2') {
      return '增值税普通发票'
    } else if (fpzldm === '41') {
      return '增值税卷票'
    } else if (fpzldm === '51') {
      return '增值税普通电子发票'
    } else if (fpzldm === '12') {
      return '机动车销售统一发票'
    }
    return ''
  }
}

``` 


