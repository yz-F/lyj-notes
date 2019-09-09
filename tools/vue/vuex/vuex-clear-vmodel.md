### vuex 控制 tab筛选项清空

- component 父组件
**传params对象，里面是存在vuex中对象。v-model值**
```
<search ref="searchParams" :selTab="selTab" :regionList="regionList" :typeList="typeList" :segmentList="segmentListData" :submitByList="submitByList" :isUpload="isUpload" :approveList="approveList" :newConceptList="newConceptList" @search="search" :params="params" @remote="remote"></search>

```

## 子组件 model 绑定params参数

```
    <!-- year -->
    ····
        <el-date-picker class="item fl"
        ref="year"
        v-if="yearShow"
        v-model="params.year"
        type="year"
        value-format="yyyy"
        placeholder="Year"
        >
        </el-date-picker>
        <!-- approver  -->
        <el-select class="item fl"
        ref="approveList"
        v-if="approveShow"
        v-model="params.approver"
        clearable
        :filterable="true"
        :remote="true"
        :remote-method="e => remoteMethod({value: e,index: index})"
        :placeholder="approveList.placeholder"
        >
        <el-option
            v-for="item in approveList.options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
        </el-option>
        </el-select>

        
    ···
```

### vuex

```
    const UPDATE_SEARCH = 'UPDATE_SEARCH'
    const state = {
        // 表格查询参数
        params: {
            region: '',
            type: '',
            segment: '',
            submitBy: '',
            year: '',
            approver: '',
            newConcept: ''
        },
    }
    const getters = {
        params: state => state.params,
    }
    const mutation = {
         [UPDATE_SEARCH] (state, { pager, params }) {
            state.pager = pager
            if (params) state.params = params
        },
    }
    consst actions = {
        // 清空参数
        clearParamsApi ({ commit }, pager) {
            debugger
            const params = {
            prd: '',
            tradeName: '',
            deliveryFormId: '',
            status: '',
            ecoStatus: '',
            mainGroupId: '',
            formulationFunctions: '',
            approverId: '',
            createUserId: ''
            }
            commit(UPDATE_SEARCH, { pager, params })
        },
    }
```