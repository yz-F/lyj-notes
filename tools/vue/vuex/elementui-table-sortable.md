### table 根据approvaltime排序
- [table sortable](https://element.eleme.io/#/zh-CN/component/table#table-column-attributes)

#### 子组件

```
  <el-table-column v-if="tabSpecialTime" sortable="custom" prop="updateTime" label="Approve time">
      <template slot-scope="item">
          <span v-if="item.row.updateTime">{{ item.row.updateTime}}</span>
      </template>
  </el-table-column>
```

#### 子组件方法

按照 desc: "approvalTime"排序，approval 为 prop

```
    sort ({ column, prop, order }) {
      let params = { desc: ['id'] }
      if (!prop || !order) {
        params = { desc: ['id'] }
      } else {
        const sortType = order === 'ascending' ? 'asc' : 'desc'
        params = { [sortType]: [prop] }
        //desc: "approvalTime"
      }
      this.sortParams = params
      this.$emit('sort-change', params)
    },  
```
#### 父组件

```
    <my-table
      v-loading="newLoading"
      :tableData="isUpload?tableUploadData.records:tableData.records"
      :selectTab="selTab"
      :pager="pager"
      :action="action"
      @denied="handleDenied"
      @refresh-upload="refresh"
      @sort-change="sortChange"
      @open-old="openOld"
      @pagerChange="pagerChange"
      @tableApproval="tableApproval"
      @tableWithdraw="tableWithdraw"
      @initTableEnable="initTableEnable"
      :isHasPermission="isUpload"
    ></my-table>

    sortChange (v) {
      this.sortParams = v
      this.search({})
    },


```
