### src\components\content\conceptsCollection\NewConcepts.vue
```
    <!-- 此处有权限下载 -->
    <a class="relate-item-link" :href="$utils.canDownload('conceptsDownload') ? $utils.baseUrl + item.urlLink : 'javascript:;'" target="_blank">
```

### src\utils\index.js
> name为跟后台协商的，有权限就下载。存在sessionStorage中的allOperationArr
```
    canDownload (name) {
        if (name && sessionStorage.allOperationArr) {
        if (sessionStorage.allOperationArr.indexOf(name) != -1) {
            return true
        } else {
            return false
        }
        }
    },

```