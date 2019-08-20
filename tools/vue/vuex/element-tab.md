### tab 

### html

```
    <div class="my-list-table-wrapper">
        <el-tabs v-model="activeNames" type="card" class="zdh-el-tabs mg-b-10" @tab-click="handleClick">
        <el-tab-pane label="Related BASF Product" name="first">
            <!-- <my-block-header mt="15px" ta="center" text="Related BASF Product">123</my-block-header> -->
            <ul class="my-list-table">
            <li @click="rowClick(item.fomltnId)" class="my-list-table-item zdh-pointer" v-for="(item, idx) in prdctListSmlrFomltns" :key="idx">
                <!-- <p class="my-name-1">{{item['fomltn' +  $utils.U($i18n.locale)]}}</p> -->
                <p class="my-name-1">{{item.tradeName}}</p>
                <p class="my-name-2">{{item['subCategory' +  $utils.U($i18n.locale)]}}</p>
                <!-- <p class="my-name-3">{{item.form_id}}</p> -->
                <p class="my-name-3">{{item.id}}</p>
                <p class="my-name-4">{{item['basfClaims' +  $utils.U($i18n.locale)]}}</p>
            </li>
            </ul>
            <div class="my-table-no-data" v-show="!prdctListSmlrFomltns || !prdctListSmlrFomltns.length">No Data</div>
        </el-tab-pane>
        <el-tab-pane label="Benchmark Formulation" name="second">
        <!-- <my-block-header mt="15px" ta="center" text="Benchmark Formulation">123</my-block-header> -->
        <ul class="my-list-table">
            <li @click="rowClick(item.fomltnId)" class="my-list-table-item zdh-pointer" v-for="(item, idx) in prdctListSmlrFomltns" :key="idx">
            <!-- <p class="my-name-1">{{item['fomltn' +  $utils.U($i18n.locale)]}}</p> -->
            <p class="my-name-1">{{item.tradeName}}</p>
            <p class="my-name-2">{{item['subCategory' +  $utils.U($i18n.locale)]}}</p>
            <!-- <p class="my-name-3">{{item.form_id}}</p> -->
            <p class="my-name-3">{{item.id}}</p>
            <p class="my-name-4">{{item['basfClaims' +  $utils.U($i18n.locale)]}}</p>
            </li>
        </ul>
        <div class="my-table-no-data" v-show="!prdctListSmlrFomltns || !prdctListSmlrFomltns.length">No Data</div>
        </el-tab-pane>
        </el-tabs>
    </div>
```

```
handleClick (tab) {
      if (this.activeNames === 'first') {
        this.getRelatedBasfProductList()
      } else {
        this.getRelatedBasfdistributeList()
      }
    },

```