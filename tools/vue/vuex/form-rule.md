### 表单值提交全不为空 some-->只要有一个
![eolink](../../../image/vue/vuex/vuex10.png)

```
    //第一步操作 form判断
    checkPage1 () {
        let list1 = ['mainImgSrc', 'title', 'updateYear', 'areaId', 'segmentId']
        let flag = true
        list1.some(item => {
        if (!this.uploadFileForm[item]) {
            flag = false
            this.$message.error('请完成所有必填项')
        }
        })
        return flag
        },
        // 判断是第几步
        checkPage (step) {
        var flag = true
        switch (step) {
        case 2: flag = this.checkPage1();
        break
        default: flag = false
        }
        return flag
        },
        // next 先判断是否全部填完再进行下一步
        next (step) {
        if (this.checkPage(step)) {
        this.active = step
        this.save ()
        this.getStepDataApi(step)
        }
    },
```
