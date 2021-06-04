### [带你体验Vue2和Vue3开发组件有什么区别](https://zhuanlan.zhihu.com/p/139590941)

- reactive(使用data)

```
import { reactive } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    return { state }
  }
}
```

- setup()方法也是可以用来操控methods的。创建声名方法其实和声名数据状态是一样的。— 我们需要先声名一个方法然后在setup()方法中返回(return)， 这样我们的组件内就可以调用这个方法了。

```
export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    const login = () => {
      // 登陆方法
    }
    return { 
      login,
      state
    }
  }
}
```

- 但是在 Vue3 生周期钩子不是全局可调用的了，需要另外从vue中引入。和刚刚引入reactive一样，生命周期的挂载钩子叫onMounted

```
import { reactive, onMounted } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    // ..

    onMounted(() => {
      console.log('组件已挂载')
    })

    // ...
  }
}

```

- Vue3 的设计模式给予开发者们按需引入需要使用的依赖包。这样一来就不需要多余的引用导致性能或者打包后太大的问题。Vue2就是有这个一直存在的问题。所以在 Vue3 使用计算属性，我们先需要在组件内引入computed。

```
import { reactive, onMounted, computed } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: '',
      lowerCaseUsername: computed(() => state.username.toLowerCase())
    })

    // ...
  }

```

- 接收 Props

> 但是在 Vue3 中，this无法直接拿到props属性，emit events（触发事件）和组件内的其他属性。不过全新的setup()方法可以接收两个参数：

1. props - 不可变的组件参数
2. context - Vue3 暴露出来的属性（emit，slots，attrs）


```
setup (props) {
    // ...

    onMounted(() => {
      console.log('title: ' + props.title)
    })

    // ...
}

```

- 事件 - Emitting Events

> 在setup()中的第二个参数content对象中就有emit，这个是和this.$emit是一样的。那么我们只要在setup()接收第二个参数中使用分解对象法取出emit就可以在setup方法中随意使用了。然后我们在login方法中编写登陆事件:

```

setup (props, { emit }) {
    // ...

    const login = () => {
      emit('login', {
        username: state.username,
        password: state.password
      })
    }

    // ...
}
```