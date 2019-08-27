
### 内部传参数要加state,解构赋值要包括号,内部传参和外部传参

```
    import { getSwiperListData } from '@/axios/api/conceptCollections.js'

    const UPDATE_SWIPERLISTDATA = 'UPDATE_SWIPERLISTDATA'

    const state = {
    swiperListData: [],
    params: {
        current: 1,
        size: 10,
        desc: ['position_id'],
        positionGroup: 'New'
    }
    }
    const getters = {
    swiperListData: state => state.swiperListData
    }

    const mutations = {
    [UPDATE_SWIPERLISTDATA] (state, swiperListData) {
        state.swiperListData = swiperListData
    }
    // [UPDATE_TABLEUPLOADDATA] (state, tableUploadData) {
    //   state.tableUploadData = tableUploadData
    // },
    // [UPDATE_TOTAL] (state, total) {
    //   state.pager = { ...state.pager, total }
    // },
    // [UPDATE_PARAMS] (state, params) {
    //   state.params = { ...state.params, params }
    // }
    }

    const actions = {
    /**
    *
    * @param {*} { commit }
    * @param {*} params
    */

    // async getSwiperListDataApi ({ commit }, params) 外部方法传参params
    // async getSwiperListDataApi ({ commit , params}) 内部方法传参params
    async getSwiperListDataApi ({ state, commit }) {
        // 内部方法传参要加state,解构赋值要包括号

        let { data: res } = await getSwiperListData({...state.params})
        // console.log(res.data);
        debugger
        commit(UPDATE_SWIPERLISTDATA, res)
        // commit(UPDATE_TOTAL, res.total)
    }
    // setParams ({ commit }, params) {
    //   commit(UPDATE_PARAMS, params)
    // }
    }

    export default {
    namespaced: true,
    state,
    getters,
    mutations,
    actions
    }


```

### 组件中获取数据

```
    export default {
    name: 'conceptCollections',
    data () {
        return {
        hotConcepts:null,
        // fomltnBeans: [
        //   {fomltnId: '1',fomltnEn: 'asdsd'},
        //   {fomltnId: '1',fomltnEn: 'tyirt'},
        //   {fomltnId: '1',fomltnEn: 'sdfg'},
        //   {fomltnId: '1',fomltnEn: 'asdsd'},
        //   {fomltnId: '1',fomltnEn: 'tfds'},
        //   {fomltnId: '1',fomltnEn: 'asdsd'},
        //   {fomltnId: '1',fomltnEn: 'rej'},
        //   {fomltnId: '1',fomltnEn: 'lsdfg'},
        //   {fomltnId: '1',fomltnEn: 'asrtuyerdsd'},
        //   {fomltnId: '1',fomltnEn: 'sfdht'},
        // ],
        // trdLstBeans: [
        //   {tradeName: 'KJDSG'},
        //   {tradeName: 'HJKSERT'},
        //   {tradeName: 'TYIUMG'},
        //   {tradeName: 'GJMGN'},
        //   {tradeName: 'KULSG'},
        //   {tradeName: 'EWRTRQYG'},
        //   {tradeName: 'RTYIDFZ'},
        //   {tradeName: 'JFGS'},
        //   {tradeName: 'TDFGH'},
        //   {tradeName: 'RETSDFG'},
        // ],
        swiperOptionTop: {
            slidesPerView: 3,
            spaceBetween: 5,
            // loop: true,
            loopedSlides: 6, //looped slides should be the same
            // slideToClickedSlide: true,
            navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
            },
            autoplay: {
            disableOnInteraction: false,
            delay: 5000,
            },
        },
        }
    },
    created () {
        this.pageInit();
    },
    computed: {
        ...mapGetters('conceptCollection', [
        'swiperListData'
        // 'newConcepts',
        // 'fomltnBeans',
        // 'trdLstBeans'
        ])
    },
    methods: {
        ...mapActions('conceptCollection', [
        'getSwiperListDataApi'
        ]),
        pageInit () {
        this.getSwiperListDataApi()
        console.log(this.swiperListData)
        this.hotConcepts = this.swiperListData.records
        },
        goDetail (id) {
        // console.log('id',id)
        this.$router.push({
            path: '/co-co/euperlan',
            query: {
            cptId: id
            }
        })
        },
        goTradeDetail(item) {
        this.$store.commit('setActiveMenuPath', '/in-se')
        this.$router.push({path: '/in-se/tr-na/detail',query: {tradeName: item.tradeName}})
        },
        goFormulationDetail(item) {
        this.$store.commit('setActiveMenuPath', '/fo-de')
        this.$router.push({
            path:'/fo-de/detail',
            query:{
            fomltnId: item.fomltnId
            }
        })
        },
    },
    components: {
        swiper,
        swiperSlide
    },
    mounted() {
        // this.conceptIndexAction2()
    }
    }
```

### vue组件获取getter数据时候，需要在store中定义getter,不然取不到值
src\store\modules\conceptCollection.js
```
    const getters = {
    pager: state => state.pager,
    param: state => state.param
    }
```
src\components\content\conceptsCollection\NewConcepts.vue
```
  computed: {
    ...mapGetters('conceptCollection', [
      'tabNewListData',
      'pager',
      'param'
      // 'newConcepts',
      // 'fomltnBeans',
      // 'trdLstBeans'
    ])
  },
```

### store中getter可以改变返回的字段的名
如果之前state里面存取的字段是 data{name:111 id:111}--map--> data{title:111,id:111}
```
    const getters = {
        subCategoryListData: state => state.subCategoryListData.map(v => {
            return { title: v.name, id: v.id }
        }),
    }
    //不需要变化的话
    const getters = {
        subCategoryListData: state => state.subCategoryListData
    }
```

### 模板{}，不加有错
```
 this.getSearchByLetterListDataApi({keyword})
```

### commit(UPLOAD_SEARCHAPPROVAL, res)
> 像字典查询出来的数据，需要更新
> 像approve操作只是做审核，没有数据查询则不需要更新

```
    /**
    *
    * approver search
    * @param {*} { commit }
    * @param {*} params
    */
    async getApprovalListApi ({ commit }, params) {
        try {
        let { data: res } = await getSearchApproveUserListData(params)
        commit(UPLOAD_SEARCHAPPROVAL, res)
        console.log(res)
        } catch (e) {
        Message.error(e)
        }
    },
    /**
    *
    * approval
    * @param {*} { commit }
    * @param {*} params
    */
    async getSearchApprovalApi ({ commit }, params) {
        try {
        await getApprovalData(params)
        } catch (e) {
        Message.error(e)
        }
    },
```
> 从组件中更新step.此处需要commit(STEP, params)

```
/**
   *
   * step
   * @param {*} {commit}
   * @param {*} params
   */
  async getStepDataApi ({ commit }, params) {
    commit(STEP, params)
  },

```