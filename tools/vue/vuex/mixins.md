### mixins
src\mixins\pager.js

```
    import commafy from '@/filters/commafy'
    const pagerTemp = {
    methods: {
        createPagerTemp (count) {
        // 千分位处理
        count = commafy(count)
        return `${this.$t('pager.total')} ${count} ${this.$t('pager.results')}`
        }
    }
    }

    export default pagerTemp

```

### vue组件使用--用于国际化总数
```
    <template>
        <div class="user-table">
        <div v-if="pager.total > 0" class="total">{{ createPagerTemp(pager.total) }}</div>
    </template>

    import pagerTemp from '@/mixins/pager'
    export default {
        name: 'table-list',
        mixins: [ pagerTemp ],
        props: {
            pager: {
            type: Object,
            default: () => {}
            },
        }
    }
```