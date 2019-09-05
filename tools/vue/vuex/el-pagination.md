### 组件

```
    <el-pagination
        v-if="pager.total>0"
        class="zdh-ta-center"
        @current-change="handleCurrentChange"
        :current-page.sync="pager.current"
        :page-size="10"
        layout="prev, pager, next, jumper"
        :total="pager.total">
    </el-pagination>
```

### vuex

```
    // 分页 总数
    const UPDATE_TOTAL = 'UPDATE_TOTAL'
    // 分页 查询
    const UPDATE_SEARCH = 'UPDATE_SEARCH'
    const state = {
        // 分页 参数
        pager: {
            current: 1,
            size: 10,
            total: 0
        },
        // 分页 查询
        <!-- pageParam: {
        }, -->
    }
    // 列表
    tabListNewParams: {
    type: 'New',
    current: 1,
    size: 10,
    desc: ['weight']
    // updateYear: '',
    // segmentId: '',
    // areaId: ''
    },
    tabListOldParams: {
        type: 'Old',
        current: 1,
        size: 10,
        desc: ['weight']
        // updateYear: '',
        // segmentId: '',
        // areaId: ''
    }

    const getters = {
        // 分页
        pager: state => state.pager,
        param: state => state.param,
    }
    const mutations = {
        // 分页
        [UPDATE_TOTAL] (state, total) {
            state.pager = { ...state.pager, total }
        },
        // 分页 查询
        [UPDATE_SEARCH] (state, { pager, pageParam }) {
            state.pager = pager
            if (pageParam) state.pageParam = pageParam
        }
    }
    const actions = {
        // 清空搜索条件
        clearSearchApi ({state, commit}) {
            const pager = {
            ...state.pager,
            current: 1
            }
            const params = {
            ...state.params,
            id: '',
            account: '',
            phone: '',
            email: '',
            roleId: '',
            dataStatus: ''
            }
            commit(UPDATE_SEARCH, { pager, params })
        },
        // 分页 设置page
        setPagerApi ({commit}, pager) {
            commit(UPDATE_SEARCH, { pager })
        },
        /**
        * tab new切换
        * @param {*} { commit }
        * @param {*} params
        */
        async getNewTabListDataApi ({ state, commit }, params) {
            commit(UPDATE_NEWLOADING, true)
            try {
            let { data: res } = await getTabListData({...state.tabListNewParams, ...params})
            commit(UPDATE_NEWTABLISTDATA, res)
            commit(UPDATE_TOTAL, res.total)
            } catch (e) {
            }
            commit(UPDATE_NEWLOADING, false)
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

### 组件方法

```
    computed: {
        ...mapGetters('conceptCollection', [
        'pager',
        'pageParam',
        ]),

    },

    methods: {
        ...mapActions('conceptCollection', [
            'getOldTabListDataApi',
            'setPagerApi'
        ]),
        // 初始化
        pageInit () {
        const searchParams = this.getParams()
        this.getTableDataApi(searchParams)
        this.getStatusDic()
        this.getRoleDataApi()
        },
        // 搜索
        search () {
        this.setPagerApi({ current: 1 })
        const searchParams = this.getParams()
        this.getTableDataApi(searchParams)
        },
        // 分页跳转
        pageChange (current) {
        this.setPagerApi({ ...this.pager, current })
        const searchParams = this.getParams()
        this.getTableDataApi(searchParams)
        },
    }
```