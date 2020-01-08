### components

// 登录时候，handle 会累加 created 里面方法，需要 destroyed

```
  destroyed () {
    ipcRenderer.removeListener('account-login-res', this.accountLoginRes)
  },
  created () {
    // 手动登录
    ipcRenderer.on('account-login-res', this.accountLoginRes)

    }
  methods: {
  accountLoginRes (event, params) {
    let myData = params
    if (myData.data && myData.data.code === 0) {
      ......
  }

```

### interface

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

  }
```
