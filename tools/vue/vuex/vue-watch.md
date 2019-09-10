### 更改data 中对象的属性 ， wactch deep

### 组件
```
    <el-checkbox v-model="row.trending">Add to new concept</el-checkbox>

    <el-row class="sort" type="flex" align="middle" :gutter="10">
        <el-col :span="6">weight: </el-col>
        <el-col>
            <!-- <el-slider v-model="row.trendingSort" :step="1.1" :disabled="!row.trending"></el-slider> -->
            <el-input-number size="mini" :disabled="!row.trending" v-model="row.trendingSort" :step="0.1" :min="0" :max="100" @change="round"></el-input-number>
        </el-col>
        <el-col>( 0.0 - 100 )</el-col>
    </el-row>


```

**点击checkbox为false时候，weight值为0**

```
row: {
        trending: false, // checkbox
        trendingSort: 0.0 // weight值
      },
watch: {
    row: {
      handler (newName, oldName) {
        if (!oldName.trending) {
          this.row.trendingSort = 0
        }
      },
      deep: true
    }
  },
```

- [watch deep等属性](https://juejin.im/post/5ae91fa76fb9a07aa7677543)