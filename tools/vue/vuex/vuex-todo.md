### src\axios\api\dictionary.js
> axios封装以及调用字典
```
  import $axios from '../config.js'

  // 公共字典url
  const GET_DICTIONARY = '/auth/dictionaries/findByParentId'

  // download权限字典url
  const GET_DOWNLOAD_AUTH = '/manage/resource/findDownAuthority'
  //axios封装
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
> vuex 调用字典
```
  // 引入字典
  import { getDicByPositiona, getDicByPositionb } from '@/axios/api/dictionary'
  //2. mutation-type:存储mutaton相关字符串常量
  
  const UPDATE_POSITION_LIST = 'UPDATE_POSITION_LIST'
  //1. state状态
  const state = {
    positionList: []
  }
  //getters对state做相关映射,getter从组件里取一些数据
  //同步操作，无异步封装
  const getters = {
  //4. => 其实就是一个函数，return了state.positionList.map...
    positionList: state => state.positionList.map(v => {
      return { label: v.name, value: v.id }
    }),

  }
  //3. mutation是一个对象，其中有很多修改方法，方法名为mutation-type,
  //第一个参数为state,第二个参数为提交mutation传的payload
  //修改状态
  const mutations = {

    [UPDATE_POSITION_LIST] (state, positionList) {
      //修改状态
      state.positionList = positionList
    }

  }
  //5. actions异步操作，对mutation进行相关封装
  //此处将两个字典 TODO add
  const actions = {
    这里的params是从外界传入的参数
    //async getSwiperListDataApi ({ commit }, params)
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
