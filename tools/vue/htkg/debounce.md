### 防抖节流

- lodash debounce

```
npm install

```

- 使用

```
import { debounce } from 'lodash'

logOutDebounce: debounce(function() {
      console.log("退出登录;")
      this.logOut()
    }, 2000),

```