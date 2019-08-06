```
<!-- Country-->
          <el-col :span="12">
          //areaId 表单提交参数
            <el-form-item label="Country:" prop="areaId">
            //v-model绑定uploadFileForm.areaId
              <el-select v-model="uploadFileForm.areaId" placeholder="请选择">
              //:value="item.value" 为提交的Id
                <el-option
                  v-for="item in countryList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>


        uploadFileForm: {
        //第一页
          title:'',
          mainImgFileId: '',
          mainImgSrc:'',
          updateYear:'',
          segmentId:'',
          areaId:'',
          fileImgSrc:'',
          fileImgFileId: '',
          fileId:'',
          fileIds:[],
        },  

```