# 基础1-5

## 1.vue中 key 值的作用

key值：用于 管理可复用的元素。因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做使 Vue 变得非常快，但是这样也不总是符合实际需求。

>2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。

#### 例如，如果你允许用户在不同的登录方式之间切换：

    <template v-if="loginType === 'username'">
    <label>Username</label>
    <input placeholder="Enter your username">
    </template>

    <template v-else>
    <label>Email</label>
    <input placeholder="Enter your email address">
    </template>

那么在上面的代码中切换loginTypeloginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，</span>不会被替换掉，仅仅是替换了它的 placeholder`。

这样也不总是符合实际需求，所以Vue为你提供了一种方式来表达这两个元素是完全独立的，不要复用它们。只需添加一个具有唯一值的 key 属性即可：

    <template v-if="loginType === 'username'">
    <label>Username</label>
    <input placeholder="Enter your username" key="username-input">
    </template>

    <template v-else>
    <label>Email</label>
    <input placeholder="Enter your email address" key="email-input">
    </template>

现在，每次切换时，输入框都将被重新渲染    