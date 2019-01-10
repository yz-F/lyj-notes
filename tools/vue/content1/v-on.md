# 基础1-4

## 1.v-on可以监听多个方法吗？
>v-on 指令常用修饰符：

v-on可以监听多个方法，例如：

    <input type="text" :value="name" @input="onInput" @focus="onFocus" @blur="onBlur" />

但是同一种事件类型的方法，只会响应第一个，例如：

            <a href="javascript:;" @click="methodsOne" @click="methodsTwo"></a>

只会响应methodsOne方法。            