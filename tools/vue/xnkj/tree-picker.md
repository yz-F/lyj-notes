### tree 选择器

```
<el-form-item label="标签" label-width="50px">
            <el-autocomplete
              @clear="clearChange"
              v-model="queryForm.tagGuidsName"
              class="inline-input"
              style="width: 230px; display: block"
              placeholder="请输入标签名称"
              :fetch-suggestions="querySearch"
              :trigger-on-focus="false"
              @blur="inputBlur"
              @focus="inputFocus"
              @change="onInputChange"
              @select="handleSelect"
            >
              <i
                slot="suffix"
                @click="handleIconClick"
                class="el-icon-search el-input__icon"
              ></i>
            </el-autocomplete>
            <el-tag
              class="employee-tag"
              v-for="indi in checkIndis"
              :key="indi.indiCode"
              closable
              @close="handleCloseCheckTagEmployeTable(indi)"
            >
              {{ indi.indiName }}
            </el-tag>
          </el-form-item>
```

### tree 样式

```

<!-- 标签选择弹窗 -->
    <xn-dialog
      :title="oneForm.title"
      width="550px"
      height="350px"
      ref="tagChooseRef"
      :show.sync="oneForm.isShow"
      :buttons="treeButtons"
      @treeButton="saveOne"
      @treeCancel="cancelLable"
    >
      <span slot="dialog-form">
        <el-tree
          :data="treeData"
          :show-checkbox="true"
          :default-expand-all="true"
          node-key="id"
          @check="treeCheck"
          class="filter-tree"
          ref="tagChooseTree"
        >
          <span class="custom-tree-node" slot-scope="{ node, data }">
            <span>{{ node.label }}</span>
          </span>
        </el-tree>

        <el-tabs type="border-card" class="employee-tab-pane-container">
          <el-tab-pane class="employee-tab-pane">
            <span slot="label">
              <el-badge
                v-if="tempCheckIndis.length > 0"
                :value="tempCheckIndis.length"
                :max="99"
              ></el-badge>
            </span>
            <el-tag
              class="employee-tag"
              v-for="indi in tempCheckIndis"
              :key="indi.indiCode"
              closable
              @close="handleCloseCheckTagEmploye(indi)"
            >
              {{ indi.indiName }}
            </el-tag>
          </el-tab-pane>
        </el-tabs>
      </span>
    </xn-dialog>
```

### methods

```
//保存按钮
    saveOne(resolve, reject) {
      this.oneForm.isShow = false;
      let tempCheckIndis = this.tempCheckIndis;
      if (
        tempCheckIndis &&
        tempCheckIndis !== null &&
        tempCheckIndis.length > 0
      ) {
        this.checkIndis = [];
        for (let tempMap of tempCheckIndis) {
          this.checkIndis.push(tempMap);
        }
      }
    },
//取消按钮

      cancelLable() {
      console.log("this.checkIndis", this.checkIndis);
      console.log("this.checkIndis", this.tempCheckIndis);
      let hasCheck = this.tempCheckIndis;
      if (hasCheck && hasCheck !== null && hasCheck.length > 0) {
        for (let temp of hasCheck) {
          console.log(temp.indiCode);
          console.log("666", temp);
          // this.tempCheckIndis.push(temp);
          this.$refs.tagChooseTree.setChecked(temp.indiCode, true, false);
          this.$refs.tagChooseTree.setCheckedKeys([]);
        }
      }
      this.checkIndis = [];
      this.oneForm.isShow = false;
    },

 //弹窗tree点击节点
     // 弹窗添加tree节点指标
    treeCheck(v1, v2) {
      // console.log("v1");
      // console.log(v1);
      // console.log("v2");
      // console.log(v2);
      console.log(v2["checkedNodes"]);
      let checkData = v2["checkedNodes"];

      let checkIndisLen = this.tempCheckIndis.length;
      if (checkIndisLen > 0) {
        this.tempCheckIndis.splice(0, checkIndisLen);
      }

      if (checkData && checkData !== null && checkData.length > 0) {
        for (let tempNode of checkData) {
          if (tempNode["indiType"] === 2) {
            let newNode = {
              indiCode: tempNode["id"],
              indiName: tempNode["label"]
            };
            this.tempCheckIndis.push(newNode);
          }
        }
      }
    },
// 标签选择弹窗删除tree节点
      // 标签选择弹窗删除tree节点指标
    handleCloseCheckTagEmploye(tab) {
      console.log(tab);
      this.$refs.tagChooseTree.setChecked(tab.indiCode, false, true);
      this.checkIndis.forEach(
        function(value, index) {
          if (value.indiCode === tab.indiCode) {
            this.checkIndis.splice(index, 1);
          }
        }.bind(this)
      );
    },
// 标签点叉

 handleCloseCheckTagEmployeTable(tab) {
      this.queryForm.tagGuidsName = "";
      this.queryForm.tagGuids = "";
      console.log(tab);
      this.$refs.tagChooseTree.setChecked(tab.indiCode, false, true);
      this.checkIndis.forEach(
        function(value, index) {
          if (value.indiCode === tab.indiCode) {
            this.checkIndis.splice(index, 1);
          }
        }.bind(this)
      );
    },

```
