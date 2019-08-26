### 父组件调用子组件内部方法
> 父组件
```
    <edit-newdialog
      :modal-append-to-body="false"
      ref="editdetail"
      @closeDialog="closeDialog"
      :isShow="dialogNewConcept"
      :detailData="detailData"
      @close-dialog="dialogNewConcept=false"
    ></edit-newdialog>

    this.$refs.editdetail.getDetailData(this.detailData)
```

> 方法定义在子组件中getDetailData

```
    // 编辑时给数据源赋值
    async getDetailData (el, type) {
      this.getStepDataApi(1)
      let params = {
        id: el.id,
        tableSource: 0,
        status: el.status
      }
      await this.getDetailApi(params)
    },

```

