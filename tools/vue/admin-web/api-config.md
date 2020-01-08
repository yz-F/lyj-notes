### axios封装 requset.js
- 请求拦截，返回拦截，以及请求头token过期判断 src\utils\request.js
```
import Vue from 'vue'
import axios from 'axios'
import store from '@/store'
import notification from 'ant-design-vue/es/notification'
import { VueAxios } from './axios'
import { ACCESS_TOKEN } from '@/store/mutation-types'

// 创建 axios 实例
const service = axios.create({
  baseURL: process.env.VUE_APP_API_BASE_URL, // api base_url
  timeout: 6000 // 请求超时时间
})

const err = (error) => {
  if (error.response) {
    const data = error.response.data
    const token = Vue.ls.get(ACCESS_TOKEN)
    if (error.response.status === 403) {
      notification.error({
        message: 'Forbidden',
        description: data.message
      })
    }
    if (error.response.status === 401 && !(data.result && data.result.isLogin)) {
      notification.error({
        message: 'Unauthorized',
        description: 'Authorization verification failed'
      })
      if (token) {
        store.dispatch('Logout').then(() => {
          setTimeout(() => {
            window.location.reload()
          }, 1500)
        })
      }
    }
  }
  return Promise.reject(error)
}

// request interceptor
service.interceptors.request.use(config => {
  const token = Vue.ls.get(ACCESS_TOKEN)
  if (token) {
    config.headers['Access-Token'] = token // 让每个请求携带自定义 token 请根据实际情况自行修改
  }
  return config
}, err)

// response interceptor
service.interceptors.response.use((response) => {
  return response.data
}, err)

const installer = {
  vm: {},
  install (Vue) {
    Vue.use(VueAxios, service)
  }
}

export {
  installer as VueAxios,
  service as axios
}

```
### axios封装详细写法
```
/* eslint-disable handle-callback-err */
import axios from 'axios'
import store from '../../renderer/store'
import { RASDecrypt } from './rsa'
import { ipcMain } from 'electron'
const service = axios.create({
  timeout: 6000 // 请求超时时间
})
let token

// 还需要加token过期的判断
service.interceptors.request.use(config => {
  token = global.LOGINTOKEN
  console.log('axios token: ', token)
  config.headers['Authorization'] = 'Bearer ' + token
  config.headers['NN-Client-Type'] = 1
  return config
})
// 还需要加token过期的判断
service.interceptors.response.use(response => {
  let code = response.data.code
  if (code === 11001) {
    token = ''
    if (global.windowObject && global.windowObject.trayWindow) {
      global.windowObject.trayWindow.send('token-expired')
      // 给主窗口发送token过期的提示
      console.info('给主窗口发送token过期的提示')
      global.windowObject.mainWindow.send('token-expired')
      global.windowObject.roomWindow.send('token-expired')
    }
  }
  // 解密数据
  if (response.data.data) {
    if (
      response.config.url.indexOf('http://test-web.nn.com/upload/image') !== -1
    ) {
      response.data.data = RASDecrypt(response.data.data)
    }
  }
  return response
})

export default service

```
###  src\api\baseURL.js
```
src\api\baseURL.js

// 开发环境的共用配置接口
const BASEURL = {
  env: 'dev',
  path: 'http://test-svr.nn.com',
  imgPath: 'http://test-svr.nn.com',
  uploadPath: 'http://test-svr.nn.com/upload/image',
  ws: 'ws:192.168.3.69:50001',
  version: 'v2',
  // IM窗口模式配置
  imSingleWindowMode: true
  // imSingleWindowMode: false
}
export function changeWS (data) {
  if (BASEURL.path === 'http://dev-svr.nn.com') {
    BASEURL.ws = `ws:${data}`
  } else if (BASEURL.path === 'https://test-svr.nn.com') {
    BASEURL.ws = `ws://${data}`
  }
}

export default BASEURL

```
### login.js
```
// import axios from 'axios'
import BASEURL from './baseURL'
import axios from './axios'
import { RSAEncrypt, RSAEncryptTmp } from './rsa'

export function initGt () {
  // 级验证接口
  let url = BASEURL.path + `/api/${BASEURL.version}/gt/preprocess?t=` + new Date().getTime()
  return axios.get(url)
}
```
### unionIndex.js
```
import { axios } from '@/utils/request'
const api = {
  plateformInfo: '/user'
}
export default api

export function getPlateformInfo (param) {
  return axios({
    url: api.user,
    method: 'get',
    params: param
  })
}
```
### 使用 .vue文件
```
import { getSmsCaptcha, get2step } from '@/api/login'

created () {
    // 表格请求方法
    // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam))
          .then(res => {
            return res.result
          })
      },
    get2step({ })
      .then(res => {
        this.requiredTwoStepCaptcha = res.result.stepCode
      })
      .catch(() => {
        this.requiredTwoStepCaptcha = false
      })
    // this.requiredTwoStepCaptcha = true
  },

  getSmsCaptcha({ mobile: values.mobile }).then(res => {
            setTimeout(hide, 2500)
            this.$notification['success']({
              message: '提示',
              description: '验证码获取成功，您的验证码为：' + res.result.captcha,
              duration: 8
            })
          }).catch(err => {
            setTimeout(hide, 1)
            clearInterval(interval)
            state.time = 60
            state.smsSendBtn = false
            this.requestFailed(err)
          })
  method:{
    requestFailed (err) {
      this.$notification['error']({
        message: '错误',
        description: ((err.response || {}).data || {}).message || '请求出现错误，请稍后再试',
        duration: 4
      })
    }
  }
```
