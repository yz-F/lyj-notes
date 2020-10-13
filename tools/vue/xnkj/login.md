### localstorage

```
import Cookies from 'js-cookie'

let TokenKey = 'accesstoken'

export function setTokenKey (key) {
  TokenKey = key
}

export function getToken () {
  return window.localStorage.getItem(TokenKey)
}

export function setToken (token) {
  return window.localStorage.setItem(TokenKey, token)
}

export function removeToken () {
  return window.localStorage.removeItem(TokenKey)
}

export function clearUserCache () {
  removeToken()
  window.localStorage.removeItem('storedRoutes')
  window.sessionStorage.removeItem('activeBaseMenuIndex')
  window.sessionStorage.removeItem('activeMenuIndex')
  window.sessionStorage.removeItem('openedMenuIndex')
  Cookies.remove('rememberMe')
}


```

### vuex

```

import Vue from 'vue'
import URL from 'sirmapp/src/api/login'
import UserApi from 'sirmapp/src/api/user'
import WFExampleAPI from 'sinitek-workflow/src/api/wfexample'
import { setTokenKey, getToken, setToken, clearUserCache } from 'sinitek-util/src/utils/auth'

export const user = {
  state: {
    token: getToken(),
    displayName: '',
    orgId: '',
    photo: '',
    taskNumber: 0,
    messageNumber: 0,
    onlineUserNumber: 0,
    introduction: '',
    roles: [],
    taskList: []
  },
  mutations: {
    SET_TOKEN: (state, token) => {
      state.token = token
    },
    SET_CURRENT_USER: (state, user) => {
      state.displayName = user.displayName
      state.orgId = user.orgId
      state.photo = user.photo
      state.taskNumber = user.taskNumber
      state.messageNumber = user.messageNumber
      state.onlineUserNumber = user.onlineUserNumber
    },
    SET_TASKLIST: (state, taskList) => {
      state.taskList = taskList
    }
  },
  actions: {
    // 变更TokenKey
    setTokenKey ({commit}, key) {
      if (typeof key === 'string') {
        setTokenKey(key)
        commit('SET_TOKEN', getToken())
      }
    },
    // 登录
    login ({commit}, data) {
      return new Promise((resolve, reject) => {
        const headers = {isRememberMe: data.isRememberMe}
        delete data.isRememberMe
        Vue.prototype.$http.post(URL.login, data, {headers}).then(result => {
          console.log(result)
          if (result.resultcode === '0') {
            const userino = result.data
            setToken(userino.accesstoken)
            commit('SET_TOKEN', userino.accesstoken)
            resolve()
          } else {
            reject(result.message)
          }
        }).catch(error => {
          reject(error)
        })
      })
    },
    // 获取用户信息 accesstoken
    getCurrentUser ({ commit }) {
      Vue.prototype.$http.get(UserApi.currentUser).then(result => {
        commit('SET_CURRENT_USER', result.data)
      })
    },
    // 获取我的事宜信息
    getTaskList ({ commit }) {
      Vue.prototype.$http.get(WFExampleAPI.APPASIDEMANAGETTASK, {searchstatus: 0, orderBy: 'et.startTime desc'}).then(result => {
        commit('SET_TASKLIST', result.data)
      })
    },
    // 用户登出 accesstoken
    logOut ({commit}) {
      return new Promise((resolve, reject) => {
        Vue.prototype.$http.post(URL.logout).then(data => {
          commit('SET_TOKEN', '')
          clearUserCache()
          resolve()
        })
      })
    },
    // 前端 登出
    fedLogOut ({ commit }) {
      return new Promise(resolve => {
        commit('SET_TOKEN', '')
        clearUserCache()
        resolve()
      }).then(() => {
        // 为了重新实例化vue-router对象 避免bug
        location.reload()
      })
    }
  }
}

```

### 
