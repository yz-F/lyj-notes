- vuex

```
import Vue from 'vue'
import http from 'sinitek-util/src/utils/http'
export const StyleIndicator = {
  state: {
    // this.$store.getters.styleIndicatorList
    styleIndicatorList: [],
    cancelChecked: [],
  },
  // 定义 getters
  getters: {
    styleIndicatorList: state => state.styleIndicatorList.map(v => {
      return {
        name: v.fundName,
        value: v.fundCode,
        date: v.fountDt,
        done: false,
        isSave: false
      }
    }),
    // 已选条数
    chooseTableDataLengthSI: state => state.styleIndicatorList.filter(v =>
      v.done === true
    ).length,
    // 表格已选择未保存的数据
    chooseTableDatasI: state => state.styleIndicatorList.filter(v =>
      v.done === true
    ),
    // 表格已选择已保存的数据
    chooseConfirmDataSI: state => state.styleIndicatorList.filter(v =>
      v.isSave === true
    ),

  },

  mutations: {
    // 修改表格列表的初始化选中状态
    INIT_CONFIRM_DATA_STATUS: (state, param) => {
      const init = state.styleIndicatorList.findIndex(x => x.value === param.id)
      if (init !== -1) {
        state.styleIndicatorList[init].done = param.done
        state.styleIndicatorList[init].isSave = param.isSave
      }
    },
    // 修改表格列表的选中状态
    CHANGE_TABLEDATA_STATUS: (state, param) => {
      const i = state.styleIndicatorList.findIndex(x => x.value === param.id)
      if (i !== -1) {
        state.styleIndicatorList[i].done = param.status
      }
    },
    // 初始化表格数据
    INIT_LIST: (state, styleIndicatorList) => {
      state.styleIndicatorList = styleIndicatorList
    },

    // 将表格数据的状态转化未确认数据的状态
    // 先把保存状态全部取消，再选中保存数据
    CHANGE_CONFIRM_DATA: (state, params) => {
      //如果有数据
      if (params.length > 0) {
        state.styleIndicatorList.forEach(element => {
          element.isSave = false
        });
        params.forEach(item => {
          const el = state.styleIndicatorList.findIndex(v => v.value === item.value)
          if (el !== -1) {
            state.styleIndicatorList[el].isSave = true
            // this.$set(this.state.styleIndicatorList[el],'isSave',true)
          }
        });
      } else {
        //如果没有数据
        state.styleIndicatorList.forEach(element => {
          element.isSave = false
        });
      }


    },
    // 取消时候，比较那些数据变化了，并切换状态
    CHANGE_CANCEL_DATA: (state, cancel) => {
      if (cancel.length > 0) {
        cancel.forEach(element => {
          const ele = state.styleIndicatorList.findIndex(v => v.value === element.value)
          if (ele !== -1) {
            state.styleIndicatorList[ele].done = false
            // this.$set(this.state.styleIndicatorList[ele],'done',false)
          }
          // this.state.styleIndicatorList.splice()
        });
      }
    },
    // 清除表格选中状态
    REMOVE_TABLE_DATA: (state, param) => {
      const i = state.styleIndicatorList.findIndex(x => x.value === param.value)
      if (i !== -1) {
        state.styleIndicatorList[i].done = false
      }
    },
    //清除确认数据选中状态
    REMOVE_CONFIRM_DATA: (state, param) => {
      const i = state.styleIndicatorList.findIndex(x => x.value === param.value)
      if (i !== -1) {
        state.styleIndicatorList[i].done = false
        state.styleIndicatorList[i].isSave = false
      }
    },
    // 清除所有状态
    SET_CLEAR: (state) => {
      state.styleIndicatorList.forEach(element => {
        element.done = false
        element.isSave = false
      });
    },
  },
  // todo add 这里的params是从外界传入的table和新的点击查询的数据
  actions: {
    getListSI({
      commit
    }, params) {
      try {
        console.log("params", params)
        http.get('/fof/api/fund/panorama/getFundInfo', params).then(result => {
          if (result.data.datalist) {
            var res = result.data.datalist.map(v => {
              return {
                // name: v.tag_name,
                // value: v.guid,
                name: v.fundName,
                value: v.fundCode,
                date: v.fountDt,
                done: false,
                isSave: false
              }
            })
          }
          commit('INIT_LIST', res)
        })
      } catch (error) {

      }
    },
  },

}


```

- template

```
 <!-- 标签信息弹窗方式修改 -->
    <xn-dialog
      :title="labelInfo.title"
      width="500px"
      height="80px"
      :show.sync="labelInfo.isShow"
    >
      <span slot="dialog-form">
        <div class="label-container" style="position:relative">
          <!--fetch-glue-tablelist-->
          <el-form class="" :model="enumForm" :inline="true">
            <el-form-item label="标签名称">
              <el-input
                @clear="clearLable"
                clearable
                v-model="enumForm.enumName"
                :maxlength="50"
                placeholder="请输入标签名称"
              />
            </el-form-item>
            <el-form-item>
              <span class="user-container header-search">
                <el-button
                  class="filter-item"
                  type="primary"
                  icon="el-icon-search"
                  @click="onQuery"
                >查询
                </el-button>
              </span>
            </el-form-item>
          </el-form>
          <el-table
            :class="[{ activeShow: treeseen, activeHide: !treeseen }]"
            ref="labelInfoTable"
            :data="this.$store.state.IndicatorIndex.list"
            height="400px"
          >
            <!-- <el-table-column width="60" align="center">
                <template slot-scope="scope">
                  <el-radio class="radio" v-model="templateRadio"  prop="clauseId" :label="scope.$index"
                            @change.native="getTemplateRow(scope.$index,scope.row)">&nbsp;
                  </el-radio>
                  <span></span>
                </template>
              </el-table-column> -->
            <!-- <el-table-column :reserve-selection="true" prop="id" type="selection" width="40"/>
              -->
            <el-table-column width="50" label="序号">
              <template slot-scope="scope">{{ scope.$index + 1 }}</template>
            </el-table-column>

            <el-table-column>
              <template slot-scope="scope">
                <el-checkbox-group v-model="scope.row.done">
                  <el-checkbox
                    @change="checked => cbTableDataChange(checked, scope.row)"
                    :checked="checked"
                  ></el-checkbox>
                </el-checkbox-group>
              </template>
            </el-table-column>
            <el-table-column
              prop="name"
              label="标签名称"
              align="center"
            ></el-table-column>
          </el-table>
          <el-table
            style="width: 90%;top: 55px;position: absolute;"
            :class="[{ showTree: ulSeen, hideTree: !ulSeen }]"
            ref="labelInfoTableQuery"
            :data="searchList"
            height="400px"
          >
            <el-table-column width="50" label="序号">
              <template slot-scope="scope">{{ scope.$index + 1 }}</template>
            </el-table-column>
            <el-table-column>
              <template slot-scope="scope">
                <el-checkbox-group v-model="scope.row.done">
                  <el-checkbox
                    @change="checked => cbTableDataChange(checked, scope.row)"
                    :checked="checked"
                  ></el-checkbox>
                </el-checkbox-group>
              </template>
            </el-table-column>
            <!-- <el-table-column :reserve-selection="true" prop="id" type="selection" width="40"/> -->
            <el-table-column
              prop="name"
              label="标签名称"
              align="left"
            ></el-table-column>
          </el-table>
          <div class="badageCon">
            {{ this.$store.getters.chooseTableDataLength }}
          </div>
          <el-tabs type="border-card">
            <!-- <el-badge :value="12" class="item"> -->
            <el-tab-pane
              label="已选标签"
              v-if="this.$store.getters.chooseTableData"
            >
              <el-tag
                v-for="tag in this.$store.getters.chooseTableData"
                :key="tag.value"
                closable
                @close="handleTableClose(tag, index)"
              >
                {{ tag.name }}
              </el-tag>
            </el-tab-pane>
            <!-- </el-badge> -->
          </el-tabs>
          <div style="float:right;margin-top:5px">
            <el-button type="primary" @click="saveVer()" size="small">
              保存
            </el-button>
            <el-button type="info" @click="cancelVer()" size="small"
            >取消
            </el-button>
          </div>
        </div>
      </span>
    </xn-dialog>
```

- method

```
import { mapGetters, mapState, mapMutations, mapActions } from "vuex";
checked: false,
        currentTableData: [],
        treeseen: true,
        ulSeen: false,
        enumForm: {
          enumName: ""
        },
        flag: false,
 ...mapMutations({
        changeTableDataStatus: "CHANGE_TABLEDATA_STATUS",
        changeConfirmData: "CHANGE_CONFIRM_DATA",
        changeCancelData: "CHANGE_CANCEL_DATA",
        removeTableData: "REMOVE_TABLE_DATA",
        removeConfirmData: "REMOVE_CONFIRM_DATA",
        initConfirmDataStatus: "INIT_CONFIRM_DATA_STATUS",
        setClear: "SET_CLEAR"
      }),
      ...mapActions({
        getList: "getList"
      }),
// 修改表格数据的选中状态并且赋予复选框新的选中状态
      cbTableDataChange(e, id) {
        this.checked = !this.checked;
        console.log(id);
        const param = {
          id: id.value,
          status: e
        };
        this.changeTableDataStatus(param);
        this.$store.commit("CHANGE_TABLEDATA_STATUS", param);
      },
      // 点击保存，相当于把list的选中状态改变
      saveVer() {
        console.log(
          "看看是啥chooseTableData",
          this.$store.getters.chooseTableData
        );
        this.changeConfirmData(this.$store.getters.chooseTableData);
        this.labelInfo.isShow = false;
      },
      // 点击取消，把取消数据的勾选状态取消
      // 取消时候，比较那些数据变化了，并切换状态
      cancelVer() {
        var cancelData = [];
        console.log(this.$store.getters);
        var that = this;
        if (
          this.$store.getters.chooseTableData.length >
          this.$store.getters.chooseConfirmData.length
        ) {
          cancelData = this.$store.getters.chooseTableData.filter(function(el) {
            return that.$store.getters.chooseConfirmData.indexOf(el) < 0;
          });
        } else {
          cancelData = this.$store.getters.chooseConfirmData.filter(function(el) {
            return that.$store.getters.chooseTableData.indexOf(el) < 0;
          });
        }
        console.log("cancelData", cancelData);
        this.changeCancelData(cancelData);
        this.labelInfo.isShow = false;
        console.log("this.$store.getters", this.$store.getters);
        console.log(
          "this.$store.state.IndicatorIndex.list",
          this.$store.state.IndicatorIndex.list
        );
      },

      // 查询搜索标签
      onQuery() {
        // this.flagQuery = false
        // this.enumForm.enumName = '';
        this.searchList = [];
        this.treeseen = false;
        this.ulSeen = true;
        if (this.enumForm.enumName === "") {
          this.searchList = this.$store.state.IndicatorIndex.list;
        } else {
          this.searchList = this.$store.state.IndicatorIndex.list.filter(v => {
            return v.name.includes(this.enumForm.enumName);
          });
        }

        // if(this.ulSeen === true){

        // }
      },
      //清空搜索标签
      clearLable() {
        this.ulSeen = false;
        this.treeseen = true;
      },
      //记录每行的key值
      // getRowKeys(row){
      //     return row.id
      // },
      // 表格标签
      handleTableClose(item, i) {
        //清除表格选中状态
        this.removeTableData(item);
      },
      handleConfirmClose(item, i) {
        //清除确认数据选中状态，以及保存状态
        this.removeConfirmData(item);
      },
      searchSelection(param) {
        this.labelInfo.isShow = true;
      },

 _this.setClear();
            // 标签初始化复制 table数据以及确认数据赋值
            data["tagArr"].forEach((item, index) => {
              let tagMap = {
                id: item["guid"],
                name: item["tag_name"],
                done: true,
                isSave: true
              };
              // 修改标签选择表格初始化选中数据的状态
              _this.initConfirmDataStatus(tagMap);
            });

```
