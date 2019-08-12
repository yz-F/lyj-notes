### vue-router tab选项卡
![eolink](../../../image/vue/vuex/vue-router.png)

### router 配置
```
{
    // Concepts Collection 详情页 容器
    path: '/co-co/no-co',
    name: 'newOldConcepts',
    component: NewOldConcepts,
    redirect: '/co-co/no-co/n-co',
    children: [{
    // tab页 New concepts list
    path: '/co-co/no-co/n-co',
    name: 'newConcepts',
    component: NewConcepts,
    meta: [{ path: '/home', name: 'breadCrumb.home' }, { path: '/co-co', name: 'breadCrumb.conceptsCollection' }, { path: null, name: 'breadCrumb.newConceptsList' }]
    }, {
    // tab页 Old concepts list
    path: '/co-co/no-co/more',
    name: 'oldConcepts',
    component: MoreConcepts,
    meta: [{ path: '/home', name: 'breadCrumb.home' }, { path: '/co-co', name: 'breadCrumb.conceptsCollection' }, { path: null, name: 'breadCrumb.oldConceptsList' }]
    }]
}
```

### parents组件

```
<template>
  <div class="comparsion">
    <div class="inline-block-header-wrapper">
      <router-link to="/co-co/no-co/n-co" tag="span" class="inline-block-header zdh-pointer">{{$t('btn.newConcepts')}}</router-link>
      <router-link to="/co-co/no-co/more" tag="span" class="inline-block-header zdh-pointer">{{$t('btn.oldConcepts')}}</router-link>
    </div>
    <router-view></router-view>
  </div>
</template>

```

### tab组件（其中之一）

```
    <template>
    <div id="moreConcepts">
        <my-panel>
        <div class="zdh-selects-container">
            <div class="select-wrapper">
            <span class="select-label font1-12-333">{{$t('txt.updateYear')}}:</span>
            <el-select clearable style="width: 130px"
                @change="conceptSearchList"
                size="mini"
                v-model="conceptSearch.updateYear"
                placeholder="Year">
                <el-option
                v-for="(item, idx) in $utils.yearrange(2010)"
                :key="idx"
                :value="item"
                :label="item"></el-option>
            </el-select>
            </div>
            <div class="select-wrapper">
            <span class="select-label font1-12-333">{{$t('txt.segment')}}:</span>
            <el-select style="width: 130px" clearable v-model="conceptSearch.segmentName" size="mini" placeholder="Segment" @change="conceptSearchList">
                <el-option
                v-for="(item, idx) in conceptSegments"
                :key="idx"
                :label="item.typename"
                :value="item.typecode"
                ></el-option>
            </el-select>
            </div>
            <div class="select-wrapper">
            <span class="select-label font1-12-333">{{$t('txt.area')}}:</span>
            <el-select style="width: 130px" clearable v-model="conceptSearch.areaName" size="mini" placeholder="Area" @change="conceptSearchList">
                <el-option
                v-for="(item, idx) in areaCodes"
                :key="idx"
                :label="item.typename"
                :value="item.typecode"
                ></el-option>
            </el-select>
            </div>
        </div>
        </my-panel>
        <div class="list-records-wrapper">
        <span>{{$t('txt.total')}}&nbsp;&nbsp;<i>{{sumCount}}</i>&nbsp;&nbsp;{{$t('txt.conceptsfounded')}}</span>
        </div>
        <div class="relate-list-container" v-loading="loading.newConceptListParams">
        <ul class="relate-list clearfix">
            <li class="relate-item" v-for="(item,idx) in conceptListBeans" :key="idx">
            <a class="relate-item-link" :href="$utils.canDownload('conceptsDownload') ? $utils.baseUrl + item.urlLink : 'javascript:;'" target="_blank">
                <p class="relate-item-header">{{item.fileName}}</p>
                <!-- <img class="relate-item-body" :src="require('../../../assets/images/concepts/'+ item.fileName +'.png')" alt=""> -->
                <img class="relate-item-body" :src="$utils.baseUrl + item.imgPath" alt="">
                <div class="relate-item-footer clearfix">
                <p class="attr-item">{{$t('txt.year')}}:<span>{{item.updateYear}}</span></p>
                <p class="attr-item">{{$t('txt.segment')}}:<span>{{item.segmentCode}}</span></p>
                <p class="attr-item">{{$t('txt.area')}}:<span>{{item.areaCode}}</span></p>
                </div>
            </a>
            </li>
        </ul>
        <el-pagination
            v-if="sumCount>0"
            class="zdh-ta-center"
            @current-change="handleCurrentChange"
            :current-page.sync="newConceptListParams.pageNo"
            :page-size="10"
            layout="prev, pager, next, jumper"
            :total="sumCount">
        </el-pagination>
        </div>
    </div>
    </template>
```