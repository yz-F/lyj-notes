### 连接别个ip
config/index.js

```
onst path = require('path')

module.exports = {
  dev: {

    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api': {
        // target: 'https://apps.chinacloudsites.cn/basf-api',
        target: 'https://192.168.8.230:8081/basf-api',
        pathRewrite: {
          '^/api': '/'
        },
        changeOrigin: true
      }
    },

```

### 获取数据
src\axios\api_urls
```
    export const url = {
  list:'/manage/concept/list'
}

```

src\axios\api\concept.js
```
    import $axios from '../config.js'
import { url } from '../api_urls/concept.js'
import {
  _get,
  _post,
  _delete,
  _put,
  _upload
} from '../request.js'
// example
// export function getTableData () {
//   return $axios({
//     url: 'xxxxxx',
//     method: 'get'
//   })
// }
// 获取basf concept列表数据
export function getTableData(params) {
  console.log(url.list)
  return $axios.post(url.list,params)
}

```

### vuex
src\store\modules\concept.js
```
    import { getTableData } from '@/axios/api/concept.js'
//表格数据
const UPDATE_TABLEDATA = 'UPDATE_TABLEDATA'
//表格总数
const UPDATE_TOTAL = 'UPDATE_TOTAL'
//参数
const UPDATE_PARAMS = 'UPDATE_PARAMS'
const state = {
  tableData: [],
  //分页参数
  pager: {
      current: 1,
      size: 10,
      total: 0
  },
  //表格查询参数
  params: {
    areaId: '',
    type:'',
    segment:'',
    position:'',
    updateYear:'',
    status:1,
    statuses:'',
    uploader:'',
  }
}

const getters = {
  tableData: state => state.tableData,
  pager: state => state.pager,
  params: state => state.params
}

const mutations = {
  [UPDATE_TABLEDATA](state, tableData) {
      state.tableData = tableData
  },
  [UPDATE_TOTAL](state, total) {
      state.pager = { ...state.pager, total }
  },
  [UPDATE_PARAMS](state,params){
      state.params = {...state.params,params}
  }
};

const actions = {
  async getTableDataApi ({ commit }, params) {
    const {status} = {...params}
    let { data: res } = await getTableData({status})
    // console.log(res.data);
    commit(UPDATE_TABLEDATA, res)
    commit(UPDATE_TOTAL, 10000)
  },

  setParams ({commit},params){
    commit('UPDATE_PARAMS',params)
  }

}

export default {
  namespaced: true,
  state,
  getters,
  mutations,
  actions
}

```