### v-model：绑定的为data中的定义的数据，而v-for为getter获取的state的值。

```
    <el-select style="width: 130px" clearable v-model="conceptSearch.segmentName" size="mini" placeholder="Segment" @change="conceptSearchList">
        <el-option
            v-for="(item, idx) in segmentList"
            :key="idx"
            :label="item.typename"
            :value="item.typecode"
        ></el-option>
    </el-select>
```

### vue 组件

```
export default {
    name: 'new-concepts',
    mixins: [ pagerTemp ],
    data () {
        return {
            conceptSearch:{
                updateYear:"",
                segmentName:"",
                areaName:"",
            }
        }
    },
    computed: {
        ...mapGetters("concept", ["segmentList"]),
    },
    //每次选择option时候触发，v-model绑定数值改变，导致data中conceptSearch跟着改变.
    methods: {
        conceptSearchList () {
            this.getNewTabListDataApi({...searchParams, ...this.conceptSearch})
        },
    }
}
```