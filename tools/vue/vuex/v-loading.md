### elementUI v-loading vuex

  ```
  const UPDATE_NEWLOADING = 'UPDATE_NEWLOADING'

  const state = {
    newLoading: false,
  }
  const getters = {
    newLoading: state => state.newLoading
  }

  const mutations = {
    [UPDATE_NEWLOADING] (state, newLoading) {
      state.newLoading = newLoading
    }
  }
  //发请求设置为true,请求后为false
  const actions = {
    async getOldTabListDataApi ({ state, commit }, params) {
      commit(UPDATE_NEWLOADING, true)
      try {
        let { data: res } = await getTabListData({...state.tabListOldParams, ...params})
        commit(UPDATE_OLDTABLISTDATA, res)
        return true
      } catch (e) {
        return false
      }
      commit(UPDATE_NEWLOADING, false)
    } 
  }
  ```




### 组件 newLoading

  ```
  ...mapGetters('concept', [
        'newLoading'
      ]),
  ```

  ```
  <div class="relate-list-container" v-loading="newLoading">
      <ul class="relate-list clearfix">
          <li class="relate-item" v-for="(item,idx) in conceptListBeans" :key="idx">
              <a class="relate-item-link" :href="$utils.canDownload('conceptsDownload') ? $utils.baseUrl + item.urlLink : 'javascript:;'" target="_blank">
              <p class="relate-item-header">{{item.fileName}}</p>
                  <!-- <img class="relate-item-body" :src="require('../../../assets/images/concepts/'+ item.fileName +'.png')" alt=""> -->
                  <img class="relate-item-body" :src="$utils.baseUrl + item.imgPath" alt="">
              <div class="relate-item-footer clearfix">
                  <p class="attr-item">{{$t('txt.year')}}:<span>{{item.updateYear}}</span></p>
                  <p class="attr-item">{{$t('txt.segment')}}:<span>{{item.segmentCode}}</span></p>
                  <p class="attr-item">{{$t('txt.area')}}:<span>{{item.areaCode}}</span></p>
              </div>
              </a>
          </li>
      </ul>
      <el-pagination
      v-if="pager.total>0"
      class="zdh-ta-center"
      @current-change="handleCurrentChange"
      :current-page.sync="pager.current"
      :page-size="10"
      layout="prev, pager, next, jumper"
      :total="pager.total">
      </el-pagination>
  </div>
  ```