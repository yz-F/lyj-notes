### 组件
>

```
    <preview
    //嵌套弹窗
      :modal-append-to-body="false"
    //数据传入  
      :tradeBeans="tradeBeans"
      :conceptFiles="conceptFiles"
      :videoFiles="videoFiles"
      :performanceFiles="performanceFiles"
      :formulations="formulations"
      :detail="preview"
      :trendFiles="trendFiles"
      :previewOrApprove="previewOrApprove"
      :isApprove="isApprove"
    //关闭弹窗  
      :isShow="dialogPreview"
      @close-dialog="dialogPreview=false"
    ></preview>
```

```
<template>
  <el-dialog
    title="Approval"
    :modal-append-to-body="false"
    width="40%"
    :visible.sync="isVisible"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <div class="dialog-content preview">
      <el-row>
        <span class="title">Name:</span>
        <span class="content">{{detail.title}}</span>
      </el-row>
      <el-row>
        <span class="title">Picture:</span>
        <img :src="detail.mainImgSrc" alt="" width="160" height="90">
      </el-row>
      <el-row>
        <span class="title">Year:</span>
        <span class="content">{{detail.updateYear}}</span>
      </el-row>
      <el-row>
        <span class="title">Language:</span>
        <span class="content">{{detail.languageIdName}}</span>
      </el-row>
      <el-row>
        <span class="title">Region/Country:</span>
        <span class="content">{{detail.areaIdName}}</span>
      </el-row>
      <el-row v-if="detail.segmentTitle">
        <span class="title">Segment:</span>
        <span class="content">{{detail.segmentTitle}}</span>
      </el-row>
      <el-row v-if="detail.type">
        <span class="title">Type:</span>
        <span class="content">{{detail.type}}</span>
      </el-row>
    </div>
    <div class="dialog-content information">
      <!-- Related Concept -->
      <el-form  ref="ruleForm" label-position="top" size="small">
        <el-row v-if="conceptFiles && conceptFiles.length">
          <el-col :span="24">
            <el-form-item label="Related Concept" prop>
              <div
                class="photoShow"
                v-for="item in conceptFiles"
                :key="item.value"
              >
                <img :src="item.imgSrc" class="imageShow" alt="">
                <div>{{item.fileTitle}}</div>
              </div>
              <!-- <add-file :isShow="dialogAddFile" @close-dialog="dialogAddFile=false"></add-file> -->
            </el-form-item>
          </el-col>
        </el-row>
      <!-- Video -->
        <el-row v-if="videoFiles && videoFiles.length">
          <el-col :span="24">
            <el-form-item label="Video" prop>
              <div
                class="photoShow"
                v-for="item in videoFiles"
                :key="item.value"
              >
                <img :src="item.imgSrc" class="imageShow" alt="">
                <div>{{item.fileTitle}}</div>
              </div>
              <!-- <add-file :isShow="dialogAddFile" @close-dialog="dialogAddFile=false"></add-file> -->
            </el-form-item>
          </el-col>
        </el-row>
        <el-row v-if="trendFiles && trendFiles.length">
          <el-col :span="24">
            <el-form-item label="Related Trend" prop>
              <div
                class="photoShow"
                v-for="item in trendFiles"
                :key="item.value"
              >
                <img :src="item.imgSrc" class="imageShow" alt="">
                <div>{{item.fileTitle}}</div>
              </div>
              <!-- <add-file :isShow="dialogAddFile" @close-dialog="dialogAddFile=false"></add-file> -->
            </el-form-item>
          </el-col>
        </el-row>
      <!-- Related Formulation  -->
        <el-row v-if="formulations && formulations.length">
          <el-col :span="24">
            <el-form-item label="Related Formulation" prop>
              <div class="tab-content">
                <div
                  class="relatived-detail"
                  v-for="item in formulations"
                  :key="item.value">
                  <div class="id-name">{{item.id}}</div>
                  <div class="id-count">{{item.title}}</div>
                </div>
              </div>
            </el-form-item>
          </el-col>
        </el-row>
      <!-- Related BASF PRODUCE  -->
        <el-row v-if="tradeBeans && tradeBeans.length">
          <el-col :span="24">
            <el-form-item label="Related BASF PRODUCE" prop>
              <div class="tab-content">
                <div
                  class="relatived-detail"
                  v-for="item in tradeBeans"
                  :key="item.value">
                  <div class="id-name">{{item.id}}</div>
                  <div class="id-count">{{item.tradeName}}</div>
                </div>
              </div>
            </el-form-item>
          </el-col>
        </el-row>
      <!-- Related Formulation  -->
        <!-- <el-row v-if="previewOrApprove ==='management-pending'">
          <el-col :span="24">
            <el-form-item label="Approval Comment" prop>
              <div class="tab-content">
                <el-input type="textarea" v-model="advice"></el-input>
              </div>
            </el-form-item>
          </el-col>
        </el-row> -->
        <!-- Claim/Performance -->
        <el-row v-if="performanceFiles && performanceFiles.length">
          <el-col :span="24">
            <el-form-item label="Claim/Performance" prop>
            <div
                class="photoShow"
                v-for="item in performanceFiles"
                :key="item.value"
              >
                <img :src="item.imgSrc" class="imageShow" alt="">
                <div>{{item.fileTitle}}</div>
              </div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>
    <!-- <div class="dialog-content dialog-footer" v-if="previewOrApprove ==='management-pending'" >
      <el-button class="draft-btn" type="danger" size="small" @click="approve('Deny')">Deny</el-button>
      <el-button class="next-btn" type="primary" size="small" @click="approve('Approve')">Approve</el-button>
    </div>
    <div class="dialog-content dialog-footer" v-if="previewOrApprove ==='upload-denied'">
      <el-button class="draft-btn" type="danger" size="small" @click="isVisible=false">OK</el-button>
    </div>
    <div class="dialog-content dialog-footer" v-if="previewOrApprove ==='management-denied'">
      <el-button class="draft-btn" type="danger" size="small" @click="isVisible=false">OK</el-button>
    </div> -->
  <el-dialog
  title="提示"
  :visible.sync="centerDialogVisible"
  width="30%"
  center>
  <span>确认此操作？</span>
  <span slot="footer" class="dialog-footer">
    <el-button @click="centerDialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="confirmSubmit">确 定</el-button>
  </span>
</el-dialog>
  </el-dialog>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  name: 'preview',
  data () {
    return {
      preview: {},
      advice: '',
      centerDialogVisible: false,
      approveOrDeny: ''
    }
  },
  props: {
    detail: {
      type: Object,
      default: () => {
        return {
          fileNames: '',
          title: '',
          source: '',
          cids: '',
          lifestyle: '',
          segment: '',
          focus: '',
          brandFocus: '',
          targetDemographicCare: '',
          year: '',
          language: '',
          region: '',
          country: '',
          keywords: '',
          imgFileId: '',
          imgFileSrc: ''
        }
      }
    },
    tradeBeans: {
      type: Array,
      default: () => []
    },
    conceptFiles: {
      type: Array,
      default: () => []
    },
    videoFiles: {
      type: Array,
      default: () => []
    },
    performanceFiles: {
      type: Array,
      default: () => []
    },
    formulations: {
      type: Array,
      default: () => []
    },
    isShow: {
      type: Boolean,
      default: false
    },
    trendFiles: {
      type: Array,
      default: () => []
    },
    previewOrApprove: {
      type: String,
      default: 'upload-denied'
    }
  },
  methods: {
    ...mapActions("concept", [
      "getApprovalListApi",
      "getTableDataApi"
    ]),

    // 点击通过与驳回
    approve (el) {
      this.approveOrDeny = el
      this.centerDialogVisible = true
      // this.getManagementPreviewDataApi();
    },
    async confirmSubmit () {
      let id = this.detail.id
      switch (this.approveOrDeny) {
        case 'Approve': {
          let status = 4
          await this.getApprovalListApi({id, status})
          break
        }
        case 'Deny': {
          let status = 2
          let advice = this.advice
          await this.getApprovalListApi({id, status, advice})
          break
        }
      }
      let paramsData = {
        size: 10,
        current: 1,
        desc: ["id"]
      }
      /** 渲染数据管理里的审核中列表 */
      await this.getTableDataApi({ ...paramsData, tableSource: 0, status: 1 })
      // 刷新审核中的总条数
      await this.getAppendingTotalDataApi(this.pager.total)
      this.$emit('close-dialog')
      this.centerDialogVisible = false
    }
  },
  computed: {
    ...mapGetters("concept", [
      // "managementPreviewList",
      "approvalList",
      "pager"
    ]),
    isVisible: {
      get () {
        return this.isShow
      },
      set (val) {
        this.$emit('close-dialog')
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
.dialog-content{
  position: relative;
  padding: 0 30px;
  >>> .el-row {
    padding: 10px 0;
    .title {
      display: inline-block;
      vertical-align: top;
      width: 120px;
      text-align: left;
    }
    img {
      border: 1px solid #ccc;
    }
  }
}
  .preview
    .row
      display flex
      padding: 10px 0
      .fileTitle{
        margin 10px
      }
      .title
        font-size:12px;
        color:rgba(0,0,0,0.8);
        width: 100px
      .content
        flex: 1
        font-size:11px;
        color:rgba(0,0,0,1);
      .green
        color: #21bdbb
  .information
    & >>> .el-form-item label
      color: rgba(33, 189, 187, 1);
      font-size: 14px;
      font-weight: bold;
    .photoShow
      float: left;
      margin: 10px;
    .imageShow
      width: 50px;
      height: 50px;
    .uploadWrap
      width: 80px;
      height: 80px;
      border: 1px solid rgba(51, 51, 51, 0.2);
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 10px;
      margin-top: 10px;
      &.imageUpload
        display: block;
        vertical-align: middle;
        margin: 0 auto;
        width: 45px;
        height: 45px;
  .dialog-content
    position: relative;
    & >>> .el-select
      width: 268px;
    & >>> .el-input
      width: 268px;
    & >>> .el-input__inner
      height: 36px;
      line-height: 36px;
    & >>> .el-form-item--small.el-form-item
      margin-bottom: 3px;
    .upload-wrap
      margin-top: 13px;
    .upload-demo
      margin-left: 48px;
      margin-top: -37px;
    & >>> .el-upload-list__item
      border: 1px solid #dcdfe6;
      height: 36px;
    .tab-content
      display: flex;
      align-items: center;
    .relatived-detail
      padding: 0 5px;
      height: 80px;
      border: 1px solid rgba(51, 51, 51, 0.2);
      border-radius: 2px;
      margin-right: 10px;
      margin-top: 10px;
      float: left;
      .id-name
        text-align: center;
        line-height: 40px;
        white-space: nowrap;
      .id-count
        text-align: center;
        line-height: 40px;
        white-space: nowrap;
  .dialog-footer
    padding-top: 30px;
    text-align: center;
    & >>> .el-button
      margin: 0 10px;
      width: 156px;
    .draft-btn
      height: 32px;
      background: rgba(255, 153, 0, 1);
      border-radius: 2px;
      font-size: 12px;
      font-weight: bold;
      line-height: 15px;
      color: rgba(255, 252, 252, 1);
    .next-btn
      height: 32px;
      background: rgba(33, 189, 187, 1);
      border-radius: 2px;
      font-size: 12px;
      font-weight: bold;
      line-height: 15px;
      color: rgba(255, 252, 252, 1);
</style>

```