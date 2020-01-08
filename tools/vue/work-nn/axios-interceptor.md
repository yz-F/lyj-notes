### axios 拦截器

- [axios 拦截器](https://hupeip.github.io/2018/10/08/axios%E6%8B%A6%E6%88%AA%E5%99%A8/)
- src\main\api\axios.js

```
// 安装 axios  npm install axios –save-dev
import axios from 'axios'
import { DB, Storage } from '../db/dbStore'
import store from '../../renderer/store'
import util from '../../renderer/utils/util'
const { session, ipcMain } = require('electron')
// 然后创建一个axios实例，这个process.env.BASE_URL在config/dev.evn.js、prod.evn.js里面进行配置：
const service = axios.create({
  timeout: 6000 // 请求超时时间
})
let token

// 请求拦截器
service.interceptors.request.use(config => {
  // 在发起请求请做一些业务处理,需要加token过期的判断
  if (global.LOGINTOKEN) {
    token = global.LOGINTOKEN
  }
  console.log('axios token: ', token)
  if (global.windowObject && global.windowObject.trayWindow) {
    global.windowObject.trayWindow.send('get-token', token)
  }

  if (token) {
    config.headers['Authorization'] = 'Bearer ' + token
  }
  return config
})
// 响应拦截器
service.interceptors.response.use(response => {
  // 对响应数据做处理,还需要加token过期的判断
  let code = response.data.code
  if (code === 11001) {
    token = ''
    if (
      global.windowObject &&
      global.windowObject.trayWindow &&
      global.windowObject.roomWindow &&
      global.windowObject.mainWindow
    ) {
      // global.windowObject.mainWindow.send('token-expired')
      // global.windowObject.roomWindow.send('token-expired')
      // global.windowObject.trayWindow.send('token-expired')
    }
  }
  return response
})
// 将axios实例暴露出去
export default service


```

- 在 main.js 中进行引用，并配置一个别名（\$ajax）来进行调用

```
import axios from '../main/api/axios'
Vue.http = Vue.prototype.$http = axios

```

- src\main\api\login.js

```
// import axios from 'axios'
import BASEURL from './baseURL'
import axios from './axios'

export function initGt () {
  // 级验证接口
  let url = BASEURL.path + '/api/v1/gt/preprocess?t=' + new Date().getTime()
  return axios.get(url)
}
```

### src\main\login\login.js

```
import { ipcMain } from 'electron'
// 实现登录相关的业务逻辑
import {
  initGt,
  codeSend,
  loginRegister,
  accountLogin,
  leigodLogin,
  checkPhoneIsBind,
  autoAccountLogin,
  bindPhoneLogin,
  findPasswordLogin,
  userInfoLogin,
  autoUserInfoLogin,
  advertiseLogin
} from '../api/login'

export default mainWindow => {
  // 级验证
  ipcMain.on('init-gt', (event, params) => {
    initGt()
      .then(res => {
        mainWindow.send('init-gt-res', res)
      })
      .catch(err => {
        mainWindow.send('init-gt-res', err)
      })
  })

  // 获取验证码
  ipcMain.on('code-send', (event, params, cb) => {
    // params.func
    codeSend(params)
      .then(res => {
        mainWindow.send('code-send-res', res, cb)
      })
      .catch(err => {
        console.log('error', err)
        mainWindow.send('code-send-res', err, cb)
      })
  })
```
