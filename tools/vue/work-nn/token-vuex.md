### 存储用户信息，全局，每个用户有特殊 id

- vuex 数据处理，storage 里面全局存储
- 存储全局信息，格式：[uuid:{coken:xxx,userinfo:xxx,setting:xxx}]
- 存储结构
  ![](../../image/vuex/work-nn001.png)

* vuex

```
import { settings } from 'cluster'
import { Storage } from '../../../main/db/dbStore'
const SAVEPUBLICUSERINFO = 'SAVEPUBLICUSERINFO'
const savePublicToken = 'savePublicToken'
const SAVESETTING = 'SAVESETTING'
const SYSTEMSET = 'SYSTEMSET'
const state = {
  publicUserInfo: {},
  token: '',
  allInfo: {}
}
const mutations = {
  [SAVEPUBLICUSERINFO] (state, data) {
    state.publicUserInfo = data
    // 将用户信息保存到内存中
    global.LOGININFO = data
  },
  [savePublicToken] (state, data) {
    state.token = data
  },
  [SYSTEMSET] (state, data) {
    if (state.allInfo[data.key]) {
      state.allInfo[data.key][data.name] = data.val
    } else {
      state.allInfo[data.key] = {}
      state.allInfo[data.key][data.name] = data.val
    }
    // 反序列化
    if (Storage.get(`${data.key}`)) {
      var res = JSON.parse(Storage.get(`${data.key}`))
      // 给userInfo赋值
      res[data.name] = data.val
      Storage.set(`${data.key}`, JSON.stringify(res))
    } else {
      Storage.set(`${data.key}`, JSON.stringify(state.allInfo[data.key]))
    }
  },
  // 将token存储在userInfo中
  [SAVESETTING] (state, data) {
    let key = data.nn_id
    if (state.allInfo[key]) {
      state.allInfo[key].token = data.token
    } else {
      state.allInfo[key] = {}
      state.allInfo[key].token = data.token
    }
    // 反序列化
    if (Storage.get(`${key}`)) {
      var res = JSON.parse(Storage.get(`${key}`))
      // 给userInfo赋值
      res.token = data
      Storage.set(`${key}`, JSON.stringify(res))
    } else {
      Storage.set(`${key}`, JSON.stringify(state.allInfo[key]))
    }
  }
}

const actions = {
  savePublicUserInfo ({ commit }, params) {
    commit('SAVEPUBLICUSERINFO', params)
  }
  // 存储全局信息，格式：[uuid:{coken:xxx,userinfo:xxx,setting:xxx}]
  // saveSettingMsg ({ commit, state }, params) {
  //   if (params && params.nn_id) {
  //     let token = {}
  //     let currentTokenMsg = {}
  //     // 此处token需要改写
  //     token = { token: params.token }
  //     token = { token: this.state.publicUserInfo }
  //     Object.assign(currentTokenMsg, token)
  //     let tokenMsg = JSON.stringify(currentTokenMsg)
  //     let key = params.nn_id
  //     let result = {}
  //     result[key] = tokenMsg
  //     let res = []
  //     res.push(result)
  //     commit('SAVESETTING', res)
  //   }
  // }
}
export default {
  state,
  mutations,
  actions
}

```

- 组件

```
methods: {
    saveTurnOn (e) {
      let param = {
        key: Storage.get('nn_id'),
        name: 'isOpenTurnON',
        val: e
      }
      this.$store.commit('SYSTEMSET', param)
      Storage.set('isOpenTurnON', e)
      }
```
