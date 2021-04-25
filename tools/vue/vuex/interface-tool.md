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

### 跨域问题

```
// const BACKEND = process.env.WEB_ADAPTER_URL || 'https://web-adapter.test.aisino.com/';

module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://web-adapter.test.aisino.com/:8080',
        ws: true,
        changeOrigin: true
      },
      '/msg_board': {
        // target: 'http://api-ibot.aitest.aisino.com/',
        target: 'http://economist-ibot.test.aisino.com/:8080',
        ws: true,
        changeOrigin: true,
        pathRewrite:{
          '^/msg_board':'/api'
        }
      },
     

    },
  },
};

```

### 获取数据
- src\axios\api_urls
  ```
      export const url = {
    uploadfile:'/auth/public/upload_file',
    }

  ```

- src\axios\api\concept.js

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
    // 获取basf uploadfile数据
    export function getUploadFileData (params) {
      console.log(url.uploadfile)
      return $axios.post(url.uploadfile,params)
    }

```

### vuex
src\store\modules\concept.js
```
    import { getUploadFileData } from '@/axios/api/concept.js'
//uploadfile接口
const UPDATE_UPLOADFILE = 'UPDATE_UPLOADFILE'



const getters = {
  uploadFileList: state => state.uploadFileList,
}
```
const mutations = {
    /***********CONCEPT UPLOADFILE**************/
  [UPDATE_UPLOADFILE](state, uploadFileList) {
    state.uploadFileList = uploadFileList
  },
};

const actions = {
  /***********CONCEPT UPLOADFIOLE**************/
  async getUploadFileDataApi({ commit },params) {
    // const {marketSegment,pid} = {...segmentParams}
    let { data: res } = await getUploadFileData(params)
    // let { data: res } = await getSegmentData({params})
    commit(UPDATE_UPLOADFILE, res)
    // commit(UPDATE_TOTAL, 10000)
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

### 组件
src\components\content\concept\editNewdialog.vue
```
<el-form-item label="Picture:" prop="imgFileSrc">
  <div class="add">
    <el-input class="input green" v-model="uploadFileForm.imgFileSrc" clearable @clear="clear('img')"></el-input>
    <el-upload class="change"
              action=""
              :before-upload="e => imgUpload('img', e)"><span>{{form.imgFileSrc ? 'change' : 'upload'}}</span></el-upload>
  </div>
</el-form-item>

import {mapGetters, mapActions} from 'vuex'

props: {
  //要提交的表单
    uploadFileForm: {
      type: Object,
      default: () => {
        return {
          //第一页
          imgFileSrc:'',
          imgFileId: '',
          conceptName:'',
          selectYear:'',
          regionCountry:'',
          segment:''
          //第二页
        }
      }
    },
}

 methods: {
    //获取列表数据
    ...mapActions("concept", [
      "getUploadFileDataApi",
    ]),
    //获取数据字典
    ...mapActions("dictionary", [
      "getSegmentsDic",
      "getCountryDic"
    ]),
    // 校验大小并上传img
    imgUpload (type,file) {
      let data = {
        file,
        path: type === 'img' ? `/trend/picture` : `/trend/${type}`
      }
      if (file.size > this.maxsize * 1024 * 1024) return this.$message.error('不能超过' + this.maxsize + 'M');
      this.loading = true;
      this.getUploadFileList(data)
        // .then(res => {
        //   console.log(res);
        // })
      debugger
    },
 }  
 // 去掉文件
    clear (type) {
      this.form[`${type}FileSrc`] = ''
      this.form[`${type}FileId`] = ''
    },
    async getUploadFileList(data) {
      await this.getUploadFileDataApi(data);
      var res = this.uploadFileList;
      this.uploadFileForm['imgFileSrc'] = res.fileSrc
      this.uploadFileForm['imgFileId'] = res.id
    },
  computed:{
    ...mapGetters("concept", ["uploadFileList"]),
    ...mapGetters("dictionary", [
      "segmentsList",
      "countryList"
    ]),
    isVisible: {
      get() {
        return this.isShow
      },
      set(val) {
        console.log(112313)
        this.$emit('close-dialog')
        Object.assign(this.form, {
          userName: '',
          password: '',
          confirmPassword: '',
          telephone: '',
          email: '',
          role: '',
          status: '',
          expert: ''
        })
      }
    }
  }
}  


```
