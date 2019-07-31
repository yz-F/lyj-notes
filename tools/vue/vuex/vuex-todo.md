### src\axios\api\dictionary.js
```
    import $axios from '../config.js'

// 公共字典url
const GET_DICTIONARY = '/auth/dictionaries/findByParentId'

// download权限字典url
const GET_DOWNLOAD_AUTH = '/manage/resource/findDownAuthority'

export const getDictionary = (id) => {
  return $axios({
    url: GET_DICTIONARY,
    method: 'post',
    data: {
      parentId: id
    }
  })
}

// 字典api


// 获取position字典 id：83 ，id:94
export function getDicByPositiona () {
  return getDictionary(83)
}
export function getDicByPositionb () {
  return getDictionary(94)
}


```

### src\store\modules\dictionary.js

```

import { getDicByPositiona, getDicByPositionb } from '@/axios/api/dictionary'

const UPDATE_POSITION_LIST = 'UPDATE_POSITION_LIST'

const state = {
  positionList: []
}

const getters = {

  positionList: state => state.positionList.map(v => {
    return { label: v.name, value: v.id }
  }),

}

const mutations = {

  [UPDATE_POSITION_LIST] (state, positionList) {
    state.positionList = positionList
  }

}

const actions = {
  async getPositionDic ({commit}) {
    if (state.positionList.length) return
    try {
      let { data: res1 } = await getDicByPositiona()
      let { data: res2 } = await getDicByPositionb()
      commit(UPDATE_POSITION_LIST, [...res1, ...res2])
    } catch (e) {}
  },
}

export default {
  namespaced: true,
  state,
  getters,
  mutations,
  actions
}


```
