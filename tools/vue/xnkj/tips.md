### el-uopload 上传文件以及回显文件

![11](../../../image/xnkj/xnkj03.png)

```
<el-upload
              :model="addForm.file"
              action="#"
              ref="adduploadRef"
              :file-list="fileList"
              class="upload-demo"
              :on-preview="handlePreview"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              :on-success="handleSuccess"
              :on-progress="handleProgress"
              :auto-upload="true"
              :multiple="true"
              :http-request="baseCheck"
              style="width: 300px"
            >
              <el-button size="small" type="primary">选择文件</el-button>
            </el-upload>
```
- methord
```
// 编辑回显，显示文件，只用给个名字。
edit(row) {
    
      this.editForm.file = [];
      if (row.fileName) {
        this.fileList = [];
        this.fileList.push({
          name: row.fileName,
          url: ""
        });
      }
      console.log("this.fileList", this.fileList);

    },
// 移除文件    
 handleRemove(file, fileList) {
      this.fileList = [];
      this.addForm.fileName = null;
      this.editForm.fileName = null;
    },    
// 只上传一个文件
handleSuccess(res, file, fileList) {
      this.addForm.fileName = true;
      this.editForm.fileName = true;
      console.log("success");
      if (fileList.length > 1) {
        fileList.splice(0, 1);
      }
      this.fileList = fileList;
    },    
// 编辑保存
// 编辑保存
    editSave(resolve, reject) {
      var that = this;
      let form = new FormData();
      if (this.fileList[0] && this.fileList[0]["raw"]) {
        form.append("file", this.fileList[0]["raw"]);
      }
      // 将前端得到的大小也传递给后端，用于验证
      // form.append("size", "999");
      // 将业务组件传递的可以接收的文件类型也传递到后端，用于验证，验证时以此基础
      // let suffixes = "";
      form.append("fundCode", this.editForm.fundCode);
      form.append("reportName", this.editForm.reportName);
      form.append("reportType", this.editForm.reportType);
      form.append("uploadDate", this.editForm.uploadDate);
      form.append("uploadUser", this.editForm.uploadUser);
      form.append("guid", this.editForm.guid);
      console.log("form", form);
      // form.append("suffixes", suffixes ? suffixes.replace(/\./g, "") : "");
      this.$refs.editFormRef.validate(valid => {
        if (valid) {
          this.$http
            .post("/fof/api/fund/report/update", form)
            .then(result => {
              console.log("111成功");
              this.$refs.edituploadRef.submit();
              if (result.data) {
                that.onQuery();
                this.saveOrEditFormEdit.isShow = false;
                // reject();
              }
            })
            .catch(result => {
              this.saveOrEditForm.isShow = false;
              this.$message.error("保存失败");
              // reject();
            });
        } else {
          // reject();
          this.saveOrEditForm.isShow = false;
        }
        // this.$refs.editFormRef.resetFields();
      });
    },    
```
