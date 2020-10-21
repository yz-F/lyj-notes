<template>
  <div class="app-container custom-tree-container">
    <!--    <xn-tree showFilter :url="treeUrl" @nodeClick="handleNodeClick"-->
    <!--             :addNode="addNodeFun"-->
    <!--             :deleteNode="delNodeFun"-->
    <!--             :editNode="editNodeFun"-->
    <!--             ref="indiTree"></xn-tree>-->

    <div class="tree-index tree-leftmenu" style="width: 350px;overflow: scroll;"    v-show="isCollapse">
      <el-form>
        <el-form-item :inline="true">
          <div style="float: left; width: 75%;">
            <el-input
              style="width: 200px"
              :inline="true"
              placeholder="指标名称"
              v-model="filterText"
            >
            </el-input>
          </div>
          <div style="float: right">
            <slot name="extend"></slot>
          </div>
        </el-form-item>
      </el-form>
      <div ref="treeDiv" class="tree-container">
        <el-tree
          :data="treeData"
          :default-expand-all="true"
          draggable
          node-key="id"
          class="tree filter-tree"
          @node-click="handleNodeClick"
          @node-drag-start="handleDragStart"
          @node-drag-enter="handleDragEnter"
          @node-drag-leave="handleDragLeave"
          @node-drag-over="handleDragOver"
          @node-drag-end="handleDragEnd"
          @node-drop="handleDrop"
          :allow-drop="allowDrop"
          :allow-drag="allowDrag"
          :filter-node-method="filterNode"
          ref="indiTree"
          :indent="0"

        >
          <span
            class="custom-tree-node"
            style="flex: inherit"
            slot-scope="{ node, data }"
          >
            <i
              v-show="data.indiType === '1'"
              class="iconfont icon-indi-folder"
              style="color:black"
            ></i>
            <!-- <i v-show="data.indiType === '2'" class="iconfont icon-indi" style="color:black;margin-right:5px" ></i> -->
            <span :title="node.label">{{ node.label }}</span>
            <span class="hover">
              <i
                v-show="data.indiType === '1'"
                class="iconfont icon-add"
                @click.stop="() => addNodeFun(node, data)"
                style="color:black"
              ></i>
              <i
                v-show="data.id !== '0'"
                class="iconfont icon-bianji"
                @click.stop="() => editNodeFun(node, data)"
                style="color:black"
              ></i>
              <i
                v-show="data.id !== '0'"
                class="iconfont icon-shanchu"
                @click.stop="() => delNodeFun(node, data)"
                style="color:black"
              ></i>
            </span>
          </span>
        </el-tree>
      </div>
    </div>
    <div class="leftMenu">
      <el-button @click="isCollapse = !isCollapse"
                 :icon="isCollapse == true ? 'el-icon-caret-right':'el-icon-caret-left'"
      >
      </el-button>
    </div>
    <div class="block" v-show="isShowEdit === '1'" >
      <div class="scrollerY blank-container">
        <el-form :model="baseIndi" ref="baseFormRef" class="index_from">
          <xn-collapse style="width: 100%;"  class="collapse1" v-model="collapseBasic1" >
              <xn-collapse-item class="collapse1_item" name="collapse1_item">
                <template slot="title" >
                  <div>指标基本信息</div>
                </template>
                <el-form
                  class="collapse1_item-form"
                  style="width: 100%;height: 90px"
                  label-width="100px">
                  <el-row>
                    <el-col :span="12">
                      <el-form-item label-width="130px" prop="indiName">
                      <span slot="label"
                      ><span style="color: red">*</span>指标名称</span
                      >
                        <el-input
                          v-model="baseIndi.name"
                          :maxlength="100"
                          :disabled="true"
                          class="submenu-dialog-input input-small indicatorInput"
                        ></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="12">
                      <el-form-item label-width="130px" prop="indiCode" class="calcType">
                      <span slot="label"
                      ><span style="color: red">*</span>指标代码</span
                      >
                        <el-input
                          v-model="baseIndi.code"
                          :maxlength="50"
                          :disabled="true"
                          class="submenu-dialog-input input-small"
                        ></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                  <el-row>
                    <el-col :span="12">
                      <div class="formNowrap">
                        <el-form-item label-width="130px" prop="calcFlag" >
                        <span slot="label"
                        ><span style="color: red">*</span>计算类型</span
                        >
                          <el-radio-group
                            v-model="baseIndi.calcFlag"
                            @change="changeCalcFalg"
                          >
                            <el-radio label="0">存储</el-radio>
                            <el-radio label="1">SQL</el-radio>
                            <el-radio label="2">python</el-radio>
                          </el-radio-group>
                          <div
                            v-show="baseIndi.calcFlag === '2'"
                            style="display: inline-block;margin-left: 23px"
                          >
                            <xn-col-button
                              @click="showSourceCodeFun()"
                              type="text"
                              size="small"
                            >查看源码</xn-col-button
                            >
                          </div>
                        </el-form-item>
                      </div>
                    </el-col>
                    <el-col :span="12">
                      <el-form-item
                        label-width="130px"
                        label="版本信息"
                        prop="versionNumber"
                        class="calcType"
                      >
                        <el-input
                          v-model="baseIndi.versionNumber"
                          :maxlength="20"
                          :disabled="true"
                          style="width: 100px"
                        ></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                  <el-row>
                    <el-col :span="12">
                      <el-form-item label-width="130px" label="查看历史版本">
                        <!--                  <span label-width="130px">查看历史版本</span>-->
                        <xn-select
                          v-model="baseIndi.versionNumber"
                          style="width: 320px"
                          class="input-small"
                          @change="changVersionNumber($event)"
                        >
                          <xn-option
                            v-for="(versionNumberitem, index) in allVersionNumber"
                            :key="index"
                            :label="versionNumberitem"
                            :value="versionNumberitem"
                          >
                            <span v-html="versionNumberitem"></span>
                          </xn-option>
                        </xn-select>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
              </xn-collapse-item>
          </xn-collapse>
          <div v-show="baseIndi.calcFlag === '3'">
            <div>
              <xn-collapse style="width: 100%" >
                <xn-collapse-item >
                  <template slot="title" >
                    <div>指标信息</div>
                  </template>
                  <el-form-item label-width="10px" prop="mainSql">
                    <el-row>
                      <el-col :span="12">
                        <el-form-item
                          label-width="130px"
                          label="指标表达式"
                          prop="indiExp"
                        >
                          <el-input
                            v-model="indiExp"
                            :maxlength="200"
                            style="width: 300px"
                          ></el-input>
                        </el-form-item>
                      </el-col>
                      <el-col :span="12">
                        <el-form-item
                          label-width="130px"
                          label="指标汇总方式"
                          prop="indiSumType"
                        >
                          <el-input
                            v-model="indiSumType"
                            :maxlength="6"
                            style="width: 300px"
                          ></el-input>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form-item>
                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div v-show="showSourceCode === 1">
            <div>
              <xn-collapse style="width: 100%"  v-model="collapseBasic6">
                <xn-collapse-item name="collapseBasic6_item">
                  <template slot="title" >
                    <div>源码</div>
                  </template>
                  <el-form-item label-width="10px" prop="mainSql" style="height: 190px">
                    <el-input
                      v-model="sourceCode"
                      :maxlength="2000"
                      :rows="10"
                      type="textarea"
                    ></el-input>
                  </el-form-item>
                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div v-show="baseIndi.calcFlag === '0'" class="edit1">
            <indicator-edit1 ref="edit1Ref"></indicator-edit1>
          </div>
          <div v-show="baseIndi.calcFlag === '1'" class="edit2">
            <indicator-edit2 ref="edit2Ref"></indicator-edit2>
          </div>
          <div v-show="baseIndi.calcFlag === '2'" class="edit3">
            <indicator-edit3 ref="edit3Ref"></indicator-edit3>
          </div>
          <div v-show="baseIndi.calcFlag !== '3'">
            <indicator-pro
              ref="indicatorProRef"
              :calcFlag="baseIndi.calcFlag"
              :indiCodeParam="baseIndi.code"
              :indiVersionNumber="baseIndi.versionNumber"
            ></indicator-pro>
          </div>
          <div v-show="baseIndi.calcFlag === '1'">
            <div>
              <xn-collapse style="width: 100%" v-model="collapseBasic4" >
                <xn-collapse-item name="collapseBasic4_item">
                  <template slot="title" >
                    <div>SQL拼接</div>
                  </template>
                  <el-form-item label-width="10px" prop="addSql">
                    <el-row
                      v-for="(addSqlTemp, addSqlIndex) in addSqlArr"
                      :key="addSqlIndex"
                    >
                      <el-col :span="12">
                        <el-form-item prop="addSqlVal">
                          <el-input
                            v-model="addSqlTemp.sqlContext"
                            :rows="4"
                            type="textarea"
                          ></el-input>
                        </el-form-item>
                      </el-col>
                      <el-col :span="6">
                        <el-form-item label-width="20px" prop="sqlParam">
                          <xn-select
                            v-model="addSqlTemp.contextType"
                            class="input-small"
                          >
                            <xn-option
                              v-for="item in addSqlParamArr"
                              :key="item.key"
                              :label="item.key"
                              :value="item.val"
                            >
                              <span v-html="item.key"></span>
                            </xn-option>
                          </xn-select>
                        </el-form-item>
                      </el-col>
                      <el-col :span="6" style="text-align:right;">
                        <xn-col-button
                          @click="delAddSql(addSqlIndex)"
                          type="text"
                          size="small"
                        >删除</xn-col-button
                        >
                      </el-col>
                    </el-row>
                  </el-form-item>
                  <el-button type="primary" @click="addSql" :loading="false"
                  style="margin-left: 15px"><span class="iconfont icon-add" ></span
                  >&nbsp添加</el-button
                  >
                  <el-button
                    type="primary"
                    @click="queryParam(1)"
                    :loading="false"
                    style="margin-left: 100px"
                  >刷新参数</el-button
                  >                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div v-show="baseIndi.calcFlag !== '3'">
            <div>
              <xn-collapse style="width: 100%"  v-model="collapseBasic2">
                <xn-collapse-item name="collapseBasic2_item">
                  <template slot="title" >
                    <div>标签信息</div>
                  </template>
                  <el-form
                    style="width: 100%;height: 20px">
                    <el-row>
                      <el-col>
                        <el-form-item label-width="100px" label="标签选择" prop="tagSel" >
                          <el-row>
                            <el-col :span="6">
                              <div v-if="this.$store.getters.chooseConfirmData">
                                <el-input style="position:relative">
                                  <i
                                    @click="searchSelection"
                                    slot="prefix"
                                    class="el-input__icon el-icon-search"
                                  ></i>
                                </el-input>
                                <el-tag
                                  v-model="baseIndi.tagSel"
                                  v-for="(tag, index) in this.$store.getters
                        .chooseConfirmData"
                                  :key="tag.value"
                                  closable
                                  @close="handleConfirmClose(tag, index)"
                                >
                                  {{ tag.name }}
                                </el-tag>
                              </div>
                            </el-col>
                          </el-row>
                          <!-- <xn-select v-model="baseIndi.tagSel" style="width: 320px">
                            <xn-option v-for="item in tagSelOption" :key="item.tag_name" :label="item.tag_name" :value="item.guid">
                              <span v-html="item.tag_name"></span>
                            </xn-option>
                          </xn-select> -->
                          <!-- <el-button type="primary" @click="addTag" :loading="false" style="margin-left: 15px"><span class="iconfont icon-add"></span>&nbsp;&nbsp;添加</el-button> -->
                        </el-form-item>
                      </el-col>
                    </el-row>

                  </el-form>
                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div v-show="baseIndi.calcFlag !== '3'">
            <div>
              <xn-collapse style="width: 100%"  v-model="collapseBasic3">
                <xn-collapse-item name="collapseBasic3_item">
                  <template slot="title" >
                    <div>附加信息</div>
                  </template>
                  <el-form
                    style="width: 100%;height: 240px"
                  >
                    <el-row>
                      <el-col span="8">
                        <el-form-item
                          label-width="100px"
                          label="默认值"
                          prop="defaultvalue"
                        >
                          <el-input
                            v-model="baseIndi.defaultvalue"
                            :maxlength="20"
                            class="submenu-dialog-input input-small"
                          ></el-input>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  <el-row>
                    <el-col span="24">
                      <el-form-item
                        label-width="100px"
                        label="算法说明"
                        prop="arithmetic"
                      >
                        <el-input
                          v-model="baseIndi.arithmetic"
                          :maxlength="200"
                          :rows="10"
                          type="textarea"
                        ></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>

                  </el-form>
                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div v-show="baseIndi.calcFlag === '1' || baseIndi.calcFlag === '2'">
            <div>
              <xn-collapse style="width: 100%"  v-model="collapseBasic5">
                <xn-collapse-item name="collapseBasic5_item">
                  <template slot="title" >
                    <div>对外接口说明</div>
                  </template>
                  <el-form
                  style="height: 425px">
                    <el-row>
                      <el-col span="24">
                        <el-form-item
                          label-width="100px"
                          label="输入"
                        >
                          <el-input
                            v-model="baseIndi.import"
                            :maxlength="200"
                            :rows="10"
                            type="textarea"
                          ></el-input>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  <el-row>
                    <el-col span="24">
                      <el-form-item
                        label-width="100px"
                        label="输出"
                      >
                        <el-input
                          v-model="baseIndi.export"
                          :maxlength="200"
                          :rows="10"
                          type="textarea"
                        ></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                  </el-form>

                </xn-collapse-item>
              </xn-collapse>
            </div>
          </div>
          <div style="margin-top: 10px;text-align: center">
            <el-button type="primary" @click="saveEdit(0)">全部保存</el-button>
            <el-button
              type="primary"
              @click="saveEdit(1)"

            >保存版本</el-button
            >
          </div>
        </el-form>
      </div>
    </div>

    <xn-dialog
      :title="addForm.title"
      width="500px"
      height="130px"
      ref="editDialogRef"
      :show.sync="addForm.isShow"
      @saveOneButton="saveOne"
      @cancelButton="cancel"
      class="dialog1"
    >
      <span slot="dialog-form">
        <div v-show="addForm.showType === '0'">
          <div style="line-height: 50px;font-size: 18px;text-align: center">
            <xn-col-button @click="addCataFun" type="text" size="small"
            >新增分类</xn-col-button
            >
          </div>
          <div style="line-height: 50px;font-size: 18px;text-align: center">
            <xn-col-button @click="addIndiFun" type="text" size="small"
            >新增指标</xn-col-button
            >
          </div>
          <div>
            <el-form class="form_Button" style="text-align: right">
              <el-form-item>
            <el-button
                       type="primary"
                       style="text-align: center;background-color: #3687AC"
                       @click="saveOne"
                        >保存</el-button>
               <el-button
                          type="primary"
                          style="text-align: center;background-color: #9AA4AA"
                          @click="cancel">取消</el-button>
          </el-form-item>
            </el-form>
          </div>
        </div>
        <div v-show="addForm.showType === '1'">
          <el-form
            :model="addCataForm"
            label-width="90px"
            :rules="checkRules2"
            ref="addCataFormRef"
          >
            <el-form-item label="分类名称" prop="cataName" style="padding-top: 30px">
              <el-input
                v-model="addCataForm.cataName"
                :maxlength="60"
                class="input-small"
              ></el-input>
            </el-form-item>
            <el-form-item style="text-align: right">
            <el-button class="filter-item"
                                    type="primary"

                                    @click="saveOne">保存
            </el-button>
               <el-button class="filter-item"
                          type="primary"
                          style="text-align: center;background-color: #9AA4AA"
                          @click="cancel">取消
            </el-button>
              <el-button class="filter-item"
                         type="primary"
                         style="text-align: center;background-color: #9AA4AA"

                         @click="backCheck">返回
            </el-button>

          </el-form-item>
          </el-form>
        </div>
        <div v-show="addForm.showType === '2'">
          <el-form
            :model="editForm"
            label-width="90px"
            :rules="checkRules"
            ref="editFormRef"
            style="padding-top: 20px"
          >
            <el-form-item label="指标代码" prop="indiCode">
              <el-input
                v-model="editForm.indiCode"
                :maxlength="50"
                class="input-small"
              ></el-input>
            </el-form-item>
            <el-form-item label="指标名称" prop="indiName">
              <el-input
                v-model="editForm.indiName"
                :maxlength="100"
                class="input-small"
              ></el-input>
            </el-form-item>
            <div>
            <el-form-item style="text-align: right;margin-bottom: 0px" >
            <el-button class="filter-item"
                       type="primary"
                       @click="saveOne">保存
            </el-button>
               <el-button class="filter-item"
                          type="primary"
                          style="text-align: center;background-color: #9AA4AA"
                          @click="cancel">取消
            </el-button>
              <el-button class="filter-item"
                         type="primary"
                         style="text-align: center;background-color: #9AA4AA"
                         @click="backCheck">返回
            </el-button>

          </el-form-item>
        </div>          </el-form>
        </div>
      </span>
    </xn-dialog>

    <xn-dialog
      :title="reNameForm.title"
      width="500px"
      height="80px"
      :show.sync="reNameForm.isShow"
      :buttons="showEditbuttons2"
      @reNameButton="reNameFun"
      @cancelButton="cancel2"
    >
      <span slot="dialog-form">
        <el-form
          :model="reNameForm"
          label-width="90px"
          :rules="checkRules1"
          ref="reNameFormRef"
        >
          <el-form-item label="新名称" prop="newName">
            <el-input v-model="reNameForm.pid" v-show="false"></el-input>
            <el-input v-model="reNameForm.indiName" v-show="false"></el-input>
            <el-input
              v-model="reNameForm.newName"
              :maxlength="reNameForm.reNameMaxLength"
              class="input-small"
            ></el-input>
          </el-form-item>

        </el-form>
      </span>
    </xn-dialog>
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

  </div>
</template>

<script>
  import { validateBlank } from "sirmapp/src/utils/validator";
  import IndicatorEdit1 from "./components/IndicatorEdit1";
  import IndicatorEdit2 from "./components/IndicatorEdit2";
  import IndicatorEdit3 from "./components/IndicatorEdit3";
  import IndicatorExpress from "./components/IndicatorExpress";
  import IndicatorPro from "./components/IndicatorPro";
  // import IndicatorEdit4 from './components/IndicatorEdit4'
  import { mapGetters, mapState, mapMutations, mapActions } from "vuex";

  const baseTreeUrl = "/indicator/api/indicator/initTree";

  export default {
    name: "IndicatorIndex",
    components: {
      // IndicatorEdit4,
      IndicatorEdit1,
      IndicatorEdit2,
      IndicatorEdit3,
      IndicatorExpress,
      IndicatorPro
    },
    watch: {
      filterText(val) {
        this.$refs.indiTree.filter(val);
      }
      //第一次初始化选择标签
      // initSearchSelection:{
      //   handler(newName, oldName){
      //     this.searchSelectionTag = true
      //   }
      // }
    },
    created() {
      // 初始化弹窗表格

      // this.$http.post('/indicator/api/indicator/getAllTagSel').then(result => {
      //   this.tagSelOption = result.data
      // })
      this.getList();
      this.$http
        .post("/indicator/api/dataSource/getAllDataSource")
        .then(result => {
          this.dataSourceSelArr = result.data;
        });
      this.initTree(true);
    },
    mounted() {
      // this.$http.post('/indicator/api/indicator/getAllTagSel').then(result => {
      //   this.tagSelOption = result.data
      //   // this.multipleSelectionList = result.data
      // })
      // this.$http.post('/indicator/api/dataSource/getAllDataSource').then(result => {
      //   this.dataSourceSelArr = result.data
      //   if (result.data.length > 0) {
      //     this.saveConForm.dataSource = result.data[0]['sourcename']
      //   }
      // })
      // this.initTree(true)
    },
    data() {
      return {
        collapseBasic1:"collapse1_item",
        collapseBasic2:'collapseBasic2_item',
        collapseBasic3:'collapseBasic3_item',
        collapseBasic4:'collapseBasic4_item',
        collapseBasic5:'collapseBasic5_item',
        collapseBasic6:'collapseBasic6_item',
        isCollapse:true,
        checked: false,
        currentTableData: [],
        treeseen: true,
        ulSeen: false,
        enumForm: {
          enumName: ""
        },
        flag: false,
        // flagQuery: false,
        // SelectionChangeQuery:[],
        // initSearchSelection:[],
        searchList: [],
        initConfirmData: [],
        initTreeClick: false,
        //判断表格SELECT是否第一次初始化
        // initDialog:true,
        //表格select选中的条数
        select_order_number: "",
        //表格select选中的id
        // select_orderId:[],
        // 标签信息
        multipleSelectionTag: false,
        // multipleSelectionList:[],
        // confirmMultipleSelectionList:[],
        // multipleSelection:[],
        multipleSelectionQuery: [],
        tagChangeData: "",
        filterText: "",
        isShowEdit: "0",
        baseCalcFlag: "",
        treeData: [],
        sourceCode: "",
        showSourceCode: 0,
        addForm: {
          title: "新增",
          isShow: false,
          showType: "0"
        },
        reNameForm: {
          title: "重命名",
          isShow: false,
          guid: "",
          nodeType: "",
          indiName: "",
          newName: "",
          pid: "",
          reNameMaxLength: 100
        },
        labelInfo: {
          title: "请选择",
          isShow: false
          // buttons:[{action: 'save', type: 'primary', event: 'saveVer', label:'保存'},
          //     {action: 'cancel', type: 'info', event: 'cancelVer'}]
        },
        addCataForm: {
          title: "重命名",
          isShow: false,
          cataName: "",
          pid: ""
        },
        showEditbuttons: [
         // { action: "save", type: "primary", event: "saveOneButton" },
         // { action: "cancel", type: "info", event: "cancelButton" },
          //{ label: "返回", type: "info", event: "backButton" }
        ],
        showEditbuttons2: [
          { action: "save", type: "primary", event: "reNameButton" },
          { action: "cancel", type: "info", event: "cancelButton" }
        ],
        checkRules: {
          indiCode: [
            { required: true, trigger: "blur", message: "指标代码不能为空" },
            { validator: validateBlank, trigger: "blur" }
          ],
          indiName: [
            { required: true, trigger: "blur", message: "指标名称不能为空" },
            { validator: validateBlank, trigger: "blur" }
          ]
        },
        checkRules2: {
          cataName: [
            { required: true, trigger: "blur", message: "分类名称不能为空" },
            { validator: validateBlank, trigger: "blur" }
          ]
        },
        checkRules1: {
          newName: [
            { required: true, trigger: "blur", message: "名称不能为空" },
            { validator: validateBlank, trigger: "blur" }
          ]
        },
        editForm: {
          catalogId: "",
          indiCode: "",
          indiName: ""
        },
        treeUrl: baseTreeUrl,
        tagSelOption: [],
        tagSelOptionQuery: [],
        baseIndi: {
          name: "",
          code: "",
          calcFlag: "0",
          tagSel: "",
          defaultvalue: "",
          arithmetic: "",
          versionNumber: "",
          import: "",
          export: "",
          calcFlagU: "1",
          skParentIndex: "",
          cClassType: "债券"
        },
        tagArr: [],
        addSqlArr: [],
        addSqlParamArr: [],
        allVersionNumber: [],
        cClassTypeArr: [
          { key: "债券", val: "债券" },
          { key: "回购", val: "回购" },
          { key: "基金", val: "基金" },
          { key: "股票", val: "股票" },
          { key: "定期存款", val: "定期存款" },
          { key: "期货", val: "期货" },
          { key: "非标", val: "非标" },
          { key: "活期存款", val: "活期存款" },
          { key: "产品费用", val: "产品费用" },
          { key: "利率互换", val: "利率互换" },
          { key: "债券借贷", val: "债券借贷" },
          { key: "存托凭证", val: "存托凭证" }
        ],
        indiExp: "",
        indiSumType: "",
        cDataTypeArr: [
          { key: "number", val: "number" },
          { key: "clob", val: "clob" },
          { key: "blob", val: "blob" },
          { key: "char", val: "char" },
          { key: "varchar", val: "varchar" },
          { key: "date", val: "date" },
          { key: "varchar2", val: "varchar2" },
          { key: "timestamp", val: "timestamp" }
        ],
        dataSourceSelArr: []
      };
    },
    // computed:{
    //   searchList:function(){
    //     if(!this.enumForm.enumName || this.enumForm.enumName === ''){
    //       return this.$store.state.IndicatorIndex.list
    //     };
    //     return this.$store.state.IndicatorIndex.list.filter(v=>{
    //       return v.name.includes(this.enumForm.enumName)
    //     })
    //   }
    // },
    mounted: function() {
      this.toggleTip();
    },
    methods: {
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

      toggleTip() {
        $(".tipTitle").each(function(i) {
          let index = i;
          $(".tipTitle")
            .eq(index)
            .click(function() {
              $(this)
                .siblings()
                .toggle();
            });
        });
      },
      initTree(isFirst) {
        this.$http.get("/indicator/api/indicator/initTree").then(result => {
          this.treeData = result.data["data"];
          if (result.data["data"].length > 0) {
            if (isFirst) {
              let oneNode = this.findFirstNode(result.data["data"]);
              if (oneNode !== null) {
                this.handleNodeClick(oneNode);
              }
            }
            this.$nextTick(function() {
              this.$refs.indiTree.filter(this.filterText);
            });
          }
        });
      },
      findFirstNode(dataArr) {
        if (dataArr !== null && dataArr.length > 0) {
          for (let data of dataArr) {
            if (data !== null) {
              if (data["indiType"] === "2") {
                return data;
              } else {
                if (data["children"] !== null && data["children"].length > 0) {
                  return this.findFirstNode(data["children"]);
                }
              }
            }
          }
        }
        return null;
      },
      cleanData() {
        // 先清除原有数据
        this.allVersionNumber = [];
        this.showSourceCode = 0;
        this.sourceCode = "";

        // this.baseIndi.code = ''
        // this.baseIndi.name = ''
        // this.baseIndi.versionNumber = ''
        // this.baseIndi.calcFlag = '0'
        this.baseIndi.defaultvalue = "";
        this.baseIndi.arithmetic = "";
        this.baseIndi.import = "";
        this.baseIndi.export = "";

        this.baseIndi.calcFlagU = "1";
        this.baseIndi.skParentIndex = "";
        this.baseIndi.cClassType = "债券";

        this.tagArr = [];
        this.addSqlArr = [];

        // 存储
        this.$refs.edit1Ref.editForm1.tableName = "";
        this.$refs.edit1Ref.editForm1.tableFieldName = "";
        if (this.$refs.edit1Ref.dataSourceSel.length > 0) {
          this.$refs.edit1Ref.editForm1.dataSource = this.$refs.edit1Ref.dataSourceSel[0][
            "sourcename"
            ];
        } else {
          this.$refs.edit1Ref.editForm1.dataSource = "";
        }

        // SQL
        if (this.$refs.edit2Ref.dataSourceSel.length > 0) {
          this.$refs.edit2Ref.editForm2.dataSource = this.$refs.edit2Ref.dataSourceSel[0][
            "sourcename"
            ];
        } else {
          this.$refs.edit2Ref.editForm2.dataSource = "";
        }
        this.$refs.edit2Ref.editForm2.mainSql = "";

        // PYTHON
        this.$refs.edit3Ref.editForm3.methodName = "";

        // 参数
        let len = this.$refs.indicatorProRef.indicatorProArr.length;
        if (len > 0) {
          this.$refs.indicatorProRef.indicatorProArr.splice(0, len);
        }
      },
      getIndiInfo(paramMap) {
        this.isShowEdit = "1";
        this.cleanData();
        this.$http
          .post("/indicator/api/indicator/getIndiInfo", paramMap, {
           // showLoading: true
          })
          .then(result => {
            let data = result["data"];
            if (data["getType"] === "error") {
              return;
            }
            this.baseCalcFlag = data["indiBase"]["calcFlag"] + "";
            this.allVersionNumber = result["data"]["allVersionNumber"];

            this.baseIndi.code = data["indiBase"]["code"];
            this.baseIndi.name = data["indiBase"]["name"];
            this.baseIndi.versionNumber = data["indiBase"]["versionNumber"];
            this.baseIndi.calcFlag = data["indiBase"]["calcFlag"] + "";
            this.baseIndi.defaultvalue = data["indiBase"]["defaultvalue"];
            this.baseIndi.arithmetic = data["indiBase"]["arithmetic"];

            // 参数
            var _this = this;
            data["propertyArr"].forEach((item, index) => {
              let propertyDefultValue1 = "";
              let propertyDefultValue2 = "";
              let pType = item["propertyDefultValueType"];
              let defultname = item["defultname"];
              let propertyDefultValueArr = [];
              if (pType === 2) {
                let proValueArr = item["propertyDefultValueArr"];
                proValueArr.forEach((proItem, proIndex) => {
                  let proArrMap = {
                    enumName: proItem["enumName"],
                    value: proItem["value"]
                  };
                  propertyDefultValueArr.push(proArrMap);
                });
                propertyDefultValue2 = item["defultvalue"];
              } else {
                propertyDefultValue1 = item["defultvalue"];
              }
              let propertyMap = {
                propertyId: item["propertyId"],
                textfield: item["textfield"],
                paramSupportLenght: item["paramSupportLenght"] + "",
                necessary: item["necessary"] + "",
                propertyDefultValueType: pType,
                propertyDefultValueArr: propertyDefultValueArr,
                propertyDefultName: defultname,
                propertyDefultValue1: propertyDefultValue1,
                propertyDefultValue2: propertyDefultValue2,
                propertyParamEnum: item["propertyParamEnum"]
              };
              this.$refs.indicatorProRef.indicatorProArr.push(propertyMap);
            });
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

            // 存储
            if (data["indiBase"]["calcFlag"] === 0) {
              if (data["indiTable"] !== null) {
                if (_this.$refs.edit1Ref) {
                  _this.$refs.edit1Ref.editForm1.tableName =
                    data["indiTable"]["tableName"];
                  _this.$refs.edit1Ref.editForm1.tableFieldName =
                    data["indiTable"]["fieldName"];
                  _this.$refs.edit1Ref.editForm1.dataSource =
                    data["indiTable"]["sourcename"];
                }
              }
            } else if (data["indiBase"]["calcFlag"] === 1) {
              if (data["indiTable"] !== null) {
                _this.$refs.edit2Ref.editForm2.dataSource =
                  data["indiTable"]["sourcename"];
              }
              let mainSqlTab = data["mainSqlTab"];
              _this.$refs.edit2Ref.editForm2.mainSql = mainSqlTab["mainSql"];
              _this.baseIndi.import = data["indiBase"]["importVal"];
              _this.baseIndi.export = data["indiBase"]["exportVal"];
              let addSqlTabArr = data["addSqlTabArr"];
              var that = _this;
              addSqlTabArr.forEach((item, index) => {
                let tempMap = {
                  sqlContext: item["sqlContext"],
                  contextType: item["contextType"],
                  showIndex: item["showIndex"]
                };
                _this.addSqlArr.push(tempMap);
              });
              that.queryParam(2);
            } else if (data["indiBase"]["calcFlag"] === 2) {
              let pythonTab = data["pythonTab"];
              _this.sourceCode = pythonTab["sourceCode"];
              _this.$refs.edit3Ref.editForm3.methodName = pythonTab["methodName"];
              _this.baseIndi.import = data["indiBase"]["importVal"];
              _this.baseIndi.export = data["indiBase"]["exportVal"];
            }
          });
      },
      handleNodeClick(data) {
        this.flag = false;
        // this.flagQuery = false
        // this.setClear()
        this.initTreeClick = true;
        // this.initDialog = true
        let indiType = data["indiType"];
        // 1表示分类 2 表示指标
        if (indiType === "2") {
          let paramMap = {
            code: data["id"],
            name: data["label"],
            versionNumber: data["versionNumber"],
            calcFlag: data["calcFlag"]
          };
          this.getIndiInfo(paramMap);
        }
      },
      addNodeFun(node) {
        let data = node["data"];
        if (data["indiType"] === "1") {
          this.addForm.isShow = true;
          this.addForm.title = "新增";
          this.addForm.showType = "0";
          /*this.showEditbuttons[0]={ action: "save", type: "primary", event: "saveOneButton" }
          this.showEditbuttons[1]=  { action: "cancel", type: "info", event: "cancelButton" }*/
          this.editForm.catalogId = data["id"];
          this.addCataForm.pid = data["id"];

          // 清除表单数据
          this.$nextTick(() => {
            this.$refs.addCataFormRef.clearValidate();
            this.$refs.editFormRef.clearValidate();
          });
        }
      },
      delNodeFun(node, data) {
        let indiType = node["data"]["indiType"];
        if (indiType === "1") {
          this.$confirm(
            "是否确认要删除 " + node["data"]["label"] + " 分类吗?"
          ).then(() => {
            this.$http
              .post("/indicator/api/indicator/delCata", node["data"]["id"])
              .then(result => {
                if (result.message !== "ok") {
                  this.$message.error(result.message);
                } else {
                  this.$message.success("删除成功");
                  this.initTree(false);
                }
              })
              .catch(() => {
                this.$message.error("删除失败");
              });
          });
        } else if (indiType === "2") {
          this.$confirm(
            "是否确认要删除 " + node["data"]["label"] + " 指标吗?"
          ).then(() => {
            this.$http
              .post("/indicator/api/indicator/delIndi", node["data"]["id"])
              .then(result => {
                if (result.message !== "ok") {
                  this.$message.error(result.message);
                } else {
                  this.$message.success("删除成功");
                  this.initTree(false);
                }
              })
              .catch(() => {
                this.$message.error("删除失败");
              });
          });
        }
      },
      editNodeFun(node) {
        let data = node["data"];
        let indiType = data["indiType"];
        this.reNameForm.isShow = true;
        this.reNameForm.title = "重命名";
        this.reNameForm.nodeType = indiType;
        if (indiType === "2") {
          this.reNameForm.reNameMaxLength = 100;
        } else {
          this.reNameForm.reNameMaxLength = 60;
        }
        this.reNameForm.indiName = data["label"];
        this.reNameForm.guid = data["id"];
        this.reNameForm.newName = data["label"];
        this.reNameForm.pid = data["pid"];
        // 清除表单数据
        this.$nextTick(() => {
          this.$refs.reNameFormRef.clearValidate();
        });
      },
      addCataFun() {
        this.addForm.title = "新增分类";
        this.addForm.showType = "1";
      },
      addIndiFun() {
        this.addForm.title = "新增指标";
        this.addForm.showType = "2";
      },
      // addTag () {
      //   if (this.baseIndi.tagSel === '') {
      //     this.$message.error('请选择标签')
      //     return
      //   }
      //   let obj = {}
      //   let checkIsHave = 0
      //   this.tagArr.forEach((item, index) => {
      //     if (item['value'] === this.baseIndi.tagSel) {
      //       checkIsHave = 1
      //     }
      //   })
      //   if (checkIsHave === 1) {
      //     this.$message.error('标签已添加')
      //     return
      //   }
      //   this.tagSelOption.find((item) => {
      //     if (item.guid === this.baseIndi.tagSel) {
      //       obj = item
      //     }
      //   })
      //   let tagTmep = {
      //     name: obj['tag_name'],
      //     value: obj['guid']
      //   }
      //   this.tagArr.push(tagTmep)
      // },
      // delTag (index) {
      //   this.tagArr.splice(index, 1)
      // },
      saveEdit(type, resolve, reject) {
        if (this.baseCalcFlag !== this.baseIndi.calcFlag) {
          let str1 = "";
          if (this.baseIndi.calcFlag === "0") {
            str1 = "存储";
          } else if (this.baseIndi.calcFlag === "1") {
            str1 = "SQL";
          } else if (this.baseIndi.calcFlag === "2") {
            str1 = "PYTHON";
          }
          this.$confirm(
            "是否将指标计算类型转换为" + str1 + "，同时原有设置将被删除！"
          )
            .then(() => {
              this.endSaveFun(type, resolve, reject);
            })
            .catch(() => {});
        } else {
          this.endSaveFun(type, resolve, reject);
        }
      },
      endSaveFun(type, resolve, reject) {
        let saveMap = {};
        saveMap["versionNumber"] = this.baseIndi.versionNumber;
        saveMap["isNewVersion"] = type;

        saveMap["nobodyIndicator"] = this.baseIndi;

        // 存储指标
        if (this.baseIndi.calcFlag === "0") {
          let regExp1 = /^[a-zA-Z][a-zA-Z0-9_]*$/;
          let tableName = this.$refs.edit1Ref.editForm1.tableName;
          let tableFieldName = this.$refs.edit1Ref.editForm1.tableFieldName;
          if (tableName === null || tableName.trim() === "") {
            this.$message.error("位置：[存储表]→[表名] 不能为空,请填写！");
            return;
          } else {
            if (!regExp1.test(tableName)) {
              this.$message.error("位置：[存储表]→[表名] 格式不正确,请修改！");
              return;
            }
          }
          if (tableFieldName === null || tableFieldName.trim() === "") {
            this.$message.error("位置：[存储表]-[字段名] 不能为空,请填写！");
            return;
          } else {
            if (!regExp1.test(tableFieldName)) {
              this.$message.error("位置：[存储表]→[字段名] 格式不正确,请修改！");
              return;
            }
          }
          saveMap["nobodyIndicatorTable"] = {
            tableName: tableName.trim(),
            fieldName: tableFieldName.trim(),
            sourcename: this.$refs.edit1Ref.editForm1.dataSource
          };
        } else if (this.baseIndi.calcFlag === "1") {
          saveMap["nobodyIndicatorTable"] = {
            sourcename: this.$refs.edit2Ref.editForm2.dataSource
          };
          saveMap["nobodyIndicator"]["importVal"] = this.baseIndi.import;
          saveMap["nobodyIndicator"]["exportVal"] = this.baseIndi.export;
          let sqlTabArr = [];
          let mainSql = this.$refs.edit2Ref.editForm2.mainSql;
          let sqlTabMap = {
            sqlContext: mainSql,
            contextType: "0",
            showIndex: 0
          };
          sqlTabArr.push(sqlTabMap);
          this.addSqlArr.forEach((addSqlItem, addSqlIndex) => {
            let sqlTabTempMap = {
              sqlContext: addSqlItem["sqlContext"],
              contextType: addSqlItem["contextType"],
              showIndex: addSqlItem["showIndex"]
            };
            sqlTabArr.push(sqlTabTempMap);
          });
          saveMap["nobodySqlTableArr"] = sqlTabArr;
        } else if (this.baseIndi.calcFlag === "2") {
          let methodName = this.$refs.edit3Ref.editForm3.methodName;
          if (methodName === null || methodName.trim() === "") {
            this.$message.error("位置：[存储表]-[方法名] 不能为空,请填写！");
            return;
          }
          saveMap["nobodyIndicator"]["importVal"] = this.baseIndi.import;
          saveMap["nobodyIndicator"]["exportVal"] = this.baseIndi.export;
          saveMap["nobodyPythonTable"] = {
            methodName: methodName,
            sourceCode: this.sourceCode
          };
        }

        // 参数
        let nobodyIndicatorPropertyArr = [];
        let cehckProMap = {};
        let cehckProMap2 = {};
        let checkProFlage = false;
        let checkProFlageVal = "";
        let regExp2 = /^[0-9]/;
        let lastNecessary = "";
        this.$refs.indicatorProRef.indicatorProArr.forEach((item, i) => {
          let pType = item["propertyDefultValueType"];
          let defultvalue = item["propertyDefultValue1"];
          let defultname = item["propertyDefultValue1"];
          let nowNecessary = item["necessary"];
          if (i > 0) {
            if (nowNecessary === "1" && lastNecessary === "2") {
              checkProFlage = true;
              checkProFlageVal =
                "位置：[参数配置]→[是否必填] 选填参数之后不能有必填参数,请修改！";
            }
            if (nowNecessary === "1" && lastNecessary === "3") {
              checkProFlage = true;
              checkProFlageVal =
                "位置：[参数配置]→[是否必填] 选填参数键值对之后不能有必填参数或选填参数,请修改！";
            }
            if (nowNecessary === "2" && lastNecessary === "3") {
              checkProFlage = true;
              checkProFlageVal =
                "位置：[参数配置]→[是否必填] 选填参数键值对之后不能有必填参数或选填参数,请修改！";
            }
          }
          lastNecessary = nowNecessary;
          if (pType === 2) {
            defultvalue = item["propertyDefultValue2"];
            let propertyDefultValueArr = item["propertyDefultValueArr"];
            propertyDefultValueArr.forEach((temp2, v) => {
              if (defultvalue === temp2["val"]) {
                defultname = temp2["key"];
              }
            });
          }
          if (item["propertyId"] === null || item["propertyId"].trim() === "") {
            checkProFlage = true;
            checkProFlageVal = "位置：[参数配置]→[参数名称] 不能为空,请填写！";
          }
          if (cehckProMap[item["propertyId"]] === 1) {
            checkProFlage = true;
            checkProFlageVal = "位置：[参数配置]→[参数名称] 有重复,请修改！";
          } else {
            cehckProMap[item["propertyId"]] = 1;
          }
          if (item["textfield"] === null || item["textfield"].trim() === "") {
            checkProFlage = true;
            checkProFlageVal = "位置：[参数配置]→[字段名称] 不能为空,请填写！";
          }
          if (regExp2.exec(item["textfield"]) !== null) {
            checkProFlage = true;
            checkProFlageVal =
              "位置：[参数配置]→[字段名称] 字段名称不能以数字开头！";
          }
          if (cehckProMap2[item["textfield"]] === 1) {
            checkProFlage = true;
            checkProFlageVal = "位置：[参数配置]→[字段名称] 有重复,请修改！";
          } else {
            cehckProMap2[item["textfield"]] = 1;
          }
          let propertyParamEnum = item["propertyParamEnum"];
          if (propertyParamEnum === "NULL") {
            propertyParamEnum = "";
          }
          let propertyMap = {
            propertyId: item["propertyId"],
            textfield: item["textfield"],
            paramSupportLenght: item["paramSupportLenght"],
            necessary: item["necessary"],
            defultvalue: defultvalue,
            defultname: defultname,
            propertyParamEnum: propertyParamEnum,
            showIndex: i + 1
          };
          nobodyIndicatorPropertyArr.push(propertyMap);
        });
        if (checkProFlage) {
          this.$message.error(checkProFlageVal);
          return;
        }
        saveMap["nobodyIndicatorPropertyArr"] = nobodyIndicatorPropertyArr;

        // 标签
        // let saveTagArr = []
        // this.tagArr.forEach((v, i) => {
        //   saveTagArr.push(v['value'])
        // })
        // saveMap['tagArr'] = saveTagArr
        let saveTagArr = [];
        this.$store.getters.chooseConfirmData.forEach((v, i) => {
          saveTagArr.push(v["value"]);
        });
        saveMap["tagArr"] = saveTagArr;
        this.$http
          .post("/indicator/api/indicator/saveIndicatorInfo", saveMap, {
            showLoading: true
          })
          .then(result => {
            if (result.message !== "ok") {
              this.$message.error(result.message);
              reject();
            } else {
              this.$message.success("保存成功");
              this.initTree(false);
              if (type === 1) {
                let paramMap = {
                  code: this.baseIndi.code,
                  name: this.baseIndi.name,
                  versionNumber:
                    this.allVersionNumber[this.allVersionNumber.length - 1] + 1,
                  calcFlag: this.baseIndi.calcFlag
                };
                this.getIndiInfo(paramMap);
              }
            }
          })
          .catch(result => {
            this.$message.error(result.message);
            reject();
          });
      },
      saveOne(resolve, reject) {
        let showType = this.addForm.showType;
        if (showType === "1") {
          this.$refs.addCataFormRef.validate(valid => {
            if (valid) {
              let saveMap = {
                name: this.addCataForm.cataName,
                parentId: this.addCataForm.pid
              };
              this.$http
                .post("/indicator/api/indicator/insCata", saveMap)
                .then(result => {
                  if (result.message !== "ok") {
                    this.$message.error(result.message);
                    reject();
                  } else {
                    this.$message.success("保存成功");
                    this.initTree(false);
                    this.cancel();
                  }
                })
                .catch(result => {
                  this.$message.error(result.message);
                  reject();
                });
            } else {
              reject();
            }
          });
        } else if (showType === "2") {
          this.$refs.editFormRef.validate(valid => {
            console.log(this.editForm);
            if (valid) {
              let saveMap = {
                name: this.editForm.indiName,
                code: this.editForm.indiCode,
                catalogId: this.editForm.catalogId
              };
              this.$http
                .post("/indicator/api/indicator/saveIndicatorBase", saveMap)
                .then(result => {
                  if (result.message !== "ok") {
                    this.$message.error(result.message);
                    reject();
                  } else {
                    this.$message.success("保存成功");
                    this.initTree(false);
                    this.cancel();
                  }
                })
                .catch(result => {
                  this.$message.error(result.message);
                  reject();
                });
            } else {
              reject();
            }
          });
        } else {
          this.$message.error("请选择新增类型");
          reject();
        }
      },
      cancel() {
        this.addForm.isShow = false;
        this.$nextTick(() => {
          this.editForm = {
            indiCode: "",
            indiName: ""
          };
          this.$refs.editFormRef.resetFields();
          this.$refs.addCataFormRef.resetFields();
        });
      },
      backCheck(resolve, reject) {
        // this.addForm.isShow = false
        this.addForm.title = "新增";
        this.addForm.showType = "0";
        reject();
      },
      reNameFun(resolve, reject) {
        let oldName = this.reNameForm.indiName;
        let newName = this.reNameForm.newName;
        if (oldName === newName) {
          this.$message.success("保存成功");
          this.cancel2();
          return;
        }
        if (newName === null || newName === "") {
          this.$message.error("新名称不能为空");
          return;
        }
        let saveMap = {
          guid: this.reNameForm.guid,
          name: this.reNameForm.newName,
          noeType: this.reNameForm.nodeType,
          pid: this.reNameForm.pid
        };
        this.$refs.reNameFormRef.validate(valid => {
          if (valid) {
            this.$http
              .post("/indicator/api/indicator/reName", saveMap)
              .then(result => {
                if (result.message !== "ok") {
                  this.$message.error(result.message);
                  reject();
                } else {
                  this.$message.success("保存成功");
                  this.initTree(false);
                  this.cancel2();
                }
              })
              .catch(result => {
                this.$message.error("重命名失败");
                reject();
              });
          } else {
            reject();
          }
        });
      },
      cancel2() {
        this.reNameForm.isShow = false;
        this.$nextTick(() => {
          this.reNameForm = {
            nodeType: "",
            indiName: "",
            newName: ""
          };
          this.$refs.reNameFormRef.resetFields();
        });
      },
      showSourceCodeFun() {
        if (this.showSourceCode === 0) {
          this.showSourceCode = 1;
        } else {
          this.showSourceCode = 0;
        }
      },
      changVersionNumber(e) {
        let paramMap = {
          code: this.baseIndi.code,
          name: this.baseIndi.name,
          versionNumber: e,
          calcFlag: this.baseIndi.calcFlag
        };
        this.getIndiInfo(paramMap);
      },
      addSql() {
        this.addSqlParamArr = [];
        let baseParam = {
          key: "默认拼接",
          val: "0"
        };
        this.addSqlParamArr.push(baseParam);
        this.$refs.indicatorProRef.indicatorProArr.forEach((item, i) => {
          let tempMap = {
            key: item["textfield"],
            val: item["textfield"]
          };
          this.addSqlParamArr.push(tempMap);
        });
        let len = this.addSqlArr.length;
        let tempMap = {
          sqlContext: "",
          contextType: "0",
          showIndex: len + 1
        };
        this.addSqlArr.push(tempMap);
      },
      delAddSql(index) {
        this.addSqlArr.splice(index, 1);
      },
      queryParam(type) {
        this.addSqlParamArr = [];
        let baseParam = {
          key: "默认拼接",
          val: "0"
        };
        this.addSqlParamArr.push(baseParam);
        this.$refs.indicatorProRef.indicatorProArr.forEach((item, i) => {
          let tempMap = {
            key: item["textfield"],
            val: item["textfield"]
          };
          this.addSqlParamArr.push(tempMap);
        });
        if (type === 1) {
          this.addSqlArr.forEach((item, i) => {
            this.addSqlArr[i].contextType = "0";
          });
        }
      },
      handleDragStart(node, ev) {
        // console.log('drag start', node.label)
      },
      handleDragEnter(draggingNode, dropNode, ev) {
        // console.log('tree drag enter: ', dropNode.label)
      },
      handleDragLeave(draggingNode, dropNode, ev) {
        // console.log('tree drag leave: ', dropNode.label)
      },
      handleDragOver(draggingNode, dropNode, ev) {
        // console.log('tree drag over: ', dropNode.label)
      },
      handleDragEnd(draggingNode, dropNode, dropType, ev) {
        console.log(draggingNode);
        console.log(
          "tree drag end: [dropType][" +
          dropType +
          "][draggingNode][" +
          draggingNode.label +
          "][dropNode][" +
          dropNode.label +
          "]"
        );

        let objId = draggingNode["data"]["id"];
        let objSort = draggingNode["data"]["sort"];
        let objType = draggingNode["data"]["indiType"];

        let targetId = dropNode["data"]["id"];
        let targetSort = dropNode["data"]["sort"];
        let targetType = dropNode["data"]["indiType"];

        let sortEntity = {
          objId: objId,
          objSort: objSort,
          targetId: targetId,
          targetSort: targetSort,
          moveType: dropType
        };
        console.log(sortEntity);
        if (targetType === "1") {
          if (objType === "1") {
            this.$http
              .post("/indicator/api/indicator/catalogMove", sortEntity)
              .then(result => {
                this.$message.success("移动成功");
              })
              .catch(result => {
                this.$message.error("移动失败");
              });
          } else {
            this.$http
              .post("/indicator/api/indicator/indicatorMoveToCatalog", sortEntity)
              .then(result => {
                this.$message.success("移动成功");
              })
              .catch(result => {
                this.$message.error("移动失败");
              });
          }
        } else {
          if (objType === "1") {
            this.$http
              .post("/indicator/api/indicator/catalogMoveToIndicator", sortEntity)
              .then(result => {
                this.$message.success("移动成功");
              })
              .catch(result => {
                this.$message.error("移动失败");
              });
          } else {
            this.$http
              .post(
                "/indicator/api/indicator/indicatorMoveToIndicator",
                sortEntity
              )
              .then(result => {
                this.$message.success("移动成功");
              })
              .catch(result => {
                this.$message.error("移动失败");
              });
          }
        }
      },
      handleDrop(draggingNode, dropNode, dropType, ev) {
        // console.log('tree drop: ', dropNode.label, dropType)
      },
      allowDrop(draggingNode, dropNode, type) {
        // console.log(dropNode.data.indiType)
        // console.log(type)
        console.log(draggingNode);
        console.log(dropNode);
        console.log(type);
        if (dropNode.data.indiType === "2" && type === "inner") {
          return false;
        }
        if (
          draggingNode.data.indiType === "2" &&
          dropNode.data.pid === "0" &&
          type !== "inner"
        ) {
          return false;
        }
        if (
          draggingNode.data.indiType === "1" &&
          dropNode.data.id === "0" &&
          type !== "inner"
        ) {
          return false;
        }
        if (draggingNode.data.indiType === "2" && dropNode.data.id === "0") {
          return false;
        }
        if (draggingNode.data.pid === dropNode.data.id && type === "inner") {
          return false;
        }
        return true;
      },
      allowDrag(draggingNode) {
        return true;
      },
      changeCalcFalg() {
        let paramMap = {
          code: this.baseIndi.code,
          name: this.baseIndi.name,
          versionNumber: this.baseIndi.versionNumber,
          calcFlag: this.baseIndi.calcFlag
        };
        this.cleanData();
        this.$http
          .post("/indicator/api/indicator/getIndiInfo", paramMap, {
            //showLoading: true
          })
          .then(result => {
            let data = result["data"];
            if (data["getType"] === "error") {
              return;
            }

            this.allVersionNumber = result["data"]["allVersionNumber"];

            if (parseInt(paramMap["calcFlag"]) !== data["indiBase"]["calcFlag"]) {
              return;
            }

            // console.log(data)
            this.baseIndi.code = data["indiBase"]["code"];
            this.baseIndi.name = data["indiBase"]["name"];
            this.baseIndi.versionNumber = data["indiBase"]["versionNumber"];
            this.baseIndi.calcFlag = data["indiBase"]["calcFlag"] + "";
            this.baseIndi.defaultvalue = data["indiBase"]["defaultvalue"];
            this.baseIndi.arithmetic = data["indiBase"]["arithmetic"];

            // 参数
            data["propertyArr"].forEach((item, index) => {
              let propertyDefultValue1 = "";
              let propertyDefultValue2 = "";
              let pType = item["propertyDefultValueType"];
              let defultname = item["defultname"];
              let propertyDefultValueArr = [];
              if (pType === 2) {
                let proValueArr = item["propertyDefultValueArr"];
                proValueArr.forEach((proItem, proIndex) => {
                  let proArrMap = {
                    key: proItem["enumName"],
                    val: proItem["value"]
                  };
                  propertyDefultValueArr.push(proArrMap);
                });
                propertyDefultValue2 = item["defultvalue"];
              } else {
                propertyDefultValue1 = item["defultvalue"];
              }
              let propertyMap = {
                propertyId: item["propertyId"],
                textfield: item["textfield"],
                paramSupportLenght: item["paramSupportLenght"] + "",
                necessary: item["necessary"] + "",
                // 待补充
                propertyDefultValueType: 1,
                propertyDefultValueArr: propertyDefultValueArr,
                propertyDefultName: defultname,
                propertyDefultValue1: propertyDefultValue1,
                propertyDefultValue2: propertyDefultValue2,
                propertyParamEnum: item["propertyParamEnum"]
              };
              this.$refs.indicatorProRef.indicatorProArr.push(propertyMap);
            });
            // 标签
            data["tagArr"].forEach((item, index) => {
              let tagMap = {
                name: item["tag_name"],
                value: item["guid"]
              };
              this.tagArr.push(tagMap);
            });

            // 存储
            if (data["indiBase"]["calcFlag"] === 0) {
              if (data["indiTable"] !== null) {
                this.$refs.edit1Ref.editForm1.tableName =
                  data["indiTable"]["tableName"];
                this.$refs.edit1Ref.editForm1.tableFieldName =
                  data["indiTable"]["fieldName"];
                this.$refs.edit1Ref.editForm1.dataSource =
                  data["indiTable"]["sourcename"];
              }
            } else if (data["indiBase"]["calcFlag"] === 1) {
              if (data["indiTable"] !== null) {
                this.$refs.edit2Ref.editForm2.dataSource =
                  data["indiTable"]["sourcename"];
              }
              let mainSqlTab = data["mainSqlTab"];
              this.$refs.edit2Ref.editForm2.mainSql = mainSqlTab["mainSql"];
              this.baseIndi.import = data["indiBase"]["importVal"];
              this.baseIndi.export = data["indiBase"]["exportVal"];
              let addSqlTabArr = data["addSqlTabArr"];
              addSqlTabArr.forEach((item, index) => {
                let tempMap = {
                  sqlContext: item["sqlContext"],
                  contextType: item["contextType"],
                  showIndex: item["showIndex"]
                };
                this.addSqlArr.push(tempMap);
              });
              this.queryParam(2);
            } else if (data["indiBase"]["calcFlag"] === 2) {
              let pythonTab = data["pythonTab"];
              this.sourceCode = pythonTab["sourceCode"];
              this.$refs.edit3Ref.editForm3.methodName = pythonTab["methodName"];
              this.baseIndi.import = data["indiBase"]["importVal"];
              this.baseIndi.export = data["indiBase"]["exportVal"];
            }
          });
      },
      filterNode(value, data) {
        if (!value) return true;
        // console.log(data)
        return data.label.toLowerCase().indexOf(value.toLowerCase()) !== -1;
      }
    }
  };
</script>
<style scoped lang="scss">

  /* 显示的文字折行 */
  .wrapstyle {
    word-wrap: break-word;
    word-break: break-all;
  }
  .custom-tree-container {
    flex: 1;
    display: flex;
  }

  .block {
    width: 100%;
/*    width: -webkit-calc(100% - 250px);
    width: -moz-calc(100% - 250px);
    width: calc(100% - 250px);*/
    margin-left: 10px;
  }

  .submenu-dialog-input {
    width: 350px;
  }

  /* icon样式 */

  .in {
    background: #333;
  }

  .in span {
    color: #fff !important;
  }

  .dib {
    display: inline-block;
  }

  .icon_lists li {
    width: 30px;
    height: 30px;
    margin: 5px;
    border: 1px solid #ccc;
    text-align: center;
    line-height: 30px;
    text-align: center;
    list-style: none !important;
    cursor: default;
  }

  .icon_lists li:hover {
    background: #333;
  }

  .icon_lists .icon {
    display: block;
    font-size: 24px;
    color: #333;
    -webkit-transition: font-size 0.25s linear, width 0.25s linear;
    -moz-transition: font-size 0.25s linear, width 0.25s linear;
    transition: font-size 0.25s linear, width 0.25s linear;
  }

  .icon_lists .icon:hover {
    color: #fff;
  }

  .tipTitle {
    margin-top: 10px;
    margin-bottom: 10px !important;
    font-size: 16px;
    padding-left: 8px;
    line-height: 1.1em;
    border-left: 3px solid rgb(255, 94, 61);
  }

  .tagLi {
    display: inline-block;
    margin-right: 5px;
  }
  .button1 {
    margin-left: 0px !important;
  }


  /* .tree-leftmenu .el-tree .el-tree-node__content .custom-tree-node span {
      padding-right: 8px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      width: 127px!important;
      display: inline !important;
  } */

  .activeShow {
    visibility: visible;
  }
  .activeHide {
    visibility: hidden;
  }
  .showTree {
    visibility: visible;
  }
  .hideTree {
    visibility: hidden;
  }
  .tree /deep/ .el-tree-node {
    position: relative;
    padding-left: 3px;
  }

  .tree /deep/ .el-tree-node__children {
    padding-left: 16px;

  }

  .tree /deep/ .el-tree-node :last-child:before {
    height: 38px;
  }

  .tree /deep/ .el-tree > .el-tree-node:before {
    border-left: none;
  }

  .tree-container /deep/ .el-tree > .el-tree-node:after {
    border-top: none;
  }

  .tree /deep/ .el-tree-node:before {
    content: "";
    left: -4px;
    position: absolute;
    right: auto;
    border-width: 1px;
  }

  .tree /deep/ .el-tree-node:after {
    content: "";
    left: -4px;
    position: absolute;
    right: auto;
    border-width: 1px;
  }

  .tree /deep/ .el-tree-node:before {
    border-left: 1px dashed #999;
    bottom: 0px;
    height: 100%;
    top: -26px;
    width: 1px;
  }
  //横着的线样式
  .tree /deep/ .el-tree-node:after {
    border-top: 1px dashed #999;
    height: 20px;
    top: 12px;
    width: 14px;
  }

  .tree-container {
    //树的parent，样式自定
  }
  .tree /deep/ .el-tree-node {
    position: relative !important;
    // background-color: blue !important;
  }
  .custom-tree-node :nth-child(4) {
    // background-color: red;
    position: absolute;
    right: -85px;
  }
  .tree /deep/.custom-tree-node .hover {
    display: none !important;
  }
  // .el-tree-node .is-expanded .is-focusable:hover .hover {

  // }
  .tree /deep/.el-tree-node__content:hover .hover {
    display: inline !important;
  }
  .tree /deep/.el-tree-node__content .is-leaf {
    width: 0px !important;
  }
  // .is-leaf .el-tree-node__expand-icon .el-icon-caret-right {
  //   display: none !important;
  // }
  .tree /deep/.el-tree-node__content {
    padding-left: 0px !important;
  }
  // tree
  .el-tree-node .is-expanded .is-focusable /deep/ .body-container {
    position: relative !important;
    overflow-x: hidden;
  }

  .el-icon-search:before {
    position: absolute;
    right: -273px;
  }
  .badageCon {
    width: 20px;
    height: 20px;
    line-height: 20px;
    background-color: #f56c6c;
    border-radius: 10px;
    color: #fff;
    display: inline-block;
    font-size: 12px;
    position: absolute;
    z-index: 9;
    left: 86px;
    text-align: center;
    white-space: nowrap;
    border: 1px solid #fff;
  }

  .tree /deep/.el-tree-node__expand-icon.expanded.el-icon-caret-right:before {
    position: absolute;
    right: 0px;
    top: -8px;
    font-size: 9px;
  }
  .tree /deep/.el-tree-node__expand-icon.expanded {
    margin-right: 10px;
  }
  .formNowrap /deep/ .el-form-item__content {
    white-space: nowrap;
  }
  .tree /deep/.el-tree-node__content {
    padding-left: 0px !important;
  }

  .tree /deep/.el-tree-node__content:hover .hover {
    display: inline !important;
  }

  .tree /deep/.el-tree-node__content .is-leaf {
    width: 0px !important;
  }

  .custom-tree-node :nth-child(4) {
    // background-color: red;
    position: absolute;
    right: -85px;
  }

  .custom-tree-node .hover {
    display: none !important;
  }

  .tree /deep/ .el-tree-node:after {
    border-top: 1px dashed #999;
    height: 20px;
    top: 12px;
    width: 14px;
  }

  .tree /deep/ .el-tree-node:before {
    border-left: 1px dashed #999;
    bottom: 0px;
    height: 100%;
    top: -26px;
    width: 1px;
  }
  //修改
  .tree-leftmenu
  .el-tree
  .el-tree-node__content
  .custom-tree-node
  span:nth-child(2) {
    padding-right: 8px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    width: 140px;
    display: inline !important;
  }
  @media screen and (min-width:768px) and (max-width:1366px){
    .calcType{
      display: inline-block;margin-left: 15px
    }
  }
  .calcType{
    display: inline-block;margin-left: 15px
  }

 >>>.el-collapse-item collapse1_item is-active{
    height: 170px !important;
  }
  >>>.el-collapse-item__wrap{
    height: 170px !important;
  }
  >>> .el-collapse collapse1{
    height: 170px !important;
  }
  .scrollerY blank\-container{
    overflow-y: auto;
    height: 100%;
    background-color: #fff;
    min-height: 60px;
    padding: 16px 19px;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
  }
  .el-dialog__footer{
    padding: 10px!important;
    padding-top: 0px!important;
    padding-bottom: 0px!important;
  }
  >>>.el-dialog__footer {
    padding: 10px!important;
    padding-top: 0px!important;
    padding-bottom: 0px!important;
    padding-right: 20px;
    text-align: right;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    border-top: 1px solid #F1F3F6;
  }
  .el-dialog /deep/ .el-dialog__footer {
    padding: 10px!important;
    padding-top: 0px!important;
    padding-bottom: 0px!important;
    padding-right: 20px;
    text-align: right;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    border-top: 1px solid #F1F3F6;
  }
  .dialog1 /deep/ .el-dialog__footer{
    padding: 10px!important;
    padding-top: 1px!important;
    padding-bottom: 1px!important;
    padding-right: 20px;
    text-align: right;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    border-top: 1px solid #F1F3F6;
  }

  /*以下四个为左侧树形菜单样式控制*/
  .leftMenu{
    /*background:#f9dbdb00;*/
    height:calc(100vh - 120px);
    line-height:calc(100vh - 120px);
    width: 20px;
    text-align: center;
    /*margin-left: 50px;*/
    /*position: absolute;*/
    /*cursor: pointer;*/
    -webkit-transform: rotate(180deg);
    transform: rotate(180deg);
    -webkit-transition: .3s;
    transition: .3s;
    -webkit-transform-origin: 50% 50%;
    transform-origin: 50% 50%;
    z-index: 1;
  }

  .leftMenu .el-button{
    width: 10px!important;
    padding: 0!important;
    height: 90px;
    border-radius:4px 0 0 4px
  }

  .leftMenu .el-button {
    color: #3687AC;
    border-color: #c3dbe6;
    background-color: #e1f0f7;
  }

  .leftMenu .el-button:hover, .el-button:focus {
    color: #3687AC;
    border-color: #c3dbe6;
    background-color: #e1f0f7;
  }

</style>
