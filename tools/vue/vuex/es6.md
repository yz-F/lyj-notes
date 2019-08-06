### 解构赋值

```
 //upload模块 撤销预览
if(type === 'upload-pending'){
    const {id} = {...row} //解构参数
    this.getApprovalListApi({id, status:3});//添加参数
}
```