# 简介

官方指定的新一代的Vuex，但作者为了尊重原作者（原作者是 Vue 核心团队成员），没有继续用Vuex命名，而是选择取名Pinina。

在Vue3中，可以选择继续使用Vuex，也可以选择体量只有1KB的Pinina，两种用法大同小异。

> 本篇文档只介绍在Vue3项目中如何使用。



# 优点

- 无论你开发的是什么项目，只要用到JavaScript你就可以用Pinina，如Vue、React、Angular、JQuery等；

- pinia中只有state、getter、action，抛弃了Vuex中的Mutation，统一在action中操作；
- pinia中action支持同步和异步，Vuex则需要区分为action和mutation；
- 良好的TypeScript支持；
- 无需再创建各个模块嵌套了，Vuex中如果数据过多，我们通常分模块来进行管理，特别麻烦，而pinia中每个store都是独立的，互相不影响；
- 体积非常小，只有1KB左右；
- Pinia同样支持插件来扩展自身功能；
- 支持服务端渲染。



# 安装

```shell
npm install pinia
# 或者使用 yarn
yarn add pinia
```



# 全局挂载

main.js

```js
import { createPinia } from "pinia"
const pinia = createPinia();

const app = createApp(App)
app.use(pinia)
```



# store

## state

### 创建

src/store/user.js

```js
import { defineStore } from 'pinia'

// 第一个参数（'users'）为该store的id，第二个参数为当前store的配置项。
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: '川普',
      age: 18,
      sex: '男',
      nationality: '中国'
    }
  }
})
```



### 读取

```vue
<template>
  <p>姓名：{{ name }}</p>
  <p>年龄：{{ age }}</p>
  <p>性别：{{ sex }}</p>
  <p>国籍：{{ nationality }}</p>
</template>


<script setup>
import { useUsersStore } from "./store/user"

const store = useUsersStore()
const { name, age, sex, nationality } = store
</script>
```



### 单个修改

```vue
<template>
  <p>年龄：{{ age }}</p>
  <button @click="clickBtn">BUTTON</button>
</template>


<script setup>
import { storeToRefs } from "pinia";
import { useUsersStore } from "./store/user"

const store = useUsersStore()

// 响应式
const { name, age, sex, nationality } = storeToRefs(store)
// 非响应式
// const { name, age, sex, nationality } = store

const clickBtn = () => {
  store.age += 1 // 非响应式情况下数据修改页面不会重新渲染
}
</script>
```



### 多个修改

在`$patch`中填写需要修改键值对。

```js
// 方式一
const clickBtn = () => {
  store.$patch({
    name: '拜登',
    age: 250
  })
}

// 方式二
const clickBtn = () => {
  store.$patch(state => {
     name: '拜登',
    state.age = 250
  })
}
```



### 覆盖

`$state`会直接覆盖对应的state对象。

```js
const clickBtn = () => {
  store.$state = {
    name: '拜登',
    isAlive: false
  }
}
```



### 重置

```vue
<template>
  <p>年龄：{{ age }}</p>
  <button @click="clickBtn">BUTTON</button>
</template>


<script setup>
import { storeToRefs } from "pinia";
import { useUsersStore } from "./store/user"

const store = useUsersStore()
const { name, age, sex, nationality } = storeToRefs(store)

store.age = 99

const clickBtn = () => {
  store.$reset()
}
</script>

```



## getters

### 创建

src/store/user.js

```js
import { defineStore } from 'pinia'

export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: '川普',
      age: 18,
      sex: '男',
      nationality: '中国'
    }
  },
  getters: {
    getAddAge: state => {
      return state.age + 100
    }
  }
})
```



### 读取

```vue
<template>
  <p>年龄：{{ age }}</p>
  <p>新年龄：{{ store.getAddAge }}</p>
</template>
```



### 修改

```vue
<template>
  <p>年龄：{{ age }}</p>
  <p>新年龄：{{ store.getAddAge }}</p>
  <button @click="clickBtn">BUTTON</button>
</template>

<script setup>
import { storeToRefs } from "pinia";
import { useUsersStore } from "./store/user"

const store = useUsersStore()
const { age } = storeToRefs(store)

const clickBtn = () => {
  store.age += 1
}
</script>
```



### 调用其他getter

```js
import { defineStore } from 'pinia'

export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: '川普',
      age: 18,
      sex: '男',
      nationality: '中国'
    }
  },
  getters: {
    getAddAge: state => {
      return state.age + 100
    },
    getNameAndAge: state => {
      // 可以使用this
      return this.name + this.getAddAge
    }
  }
})
```



### 自定义传参

```js
  getters: {
    getCustomName: state => {
      return str => str + state.name
    }
  }
```

```vue
<template>
  <p>姓名：{{ name }}</p>
  <!-- 
    输出：中国川普
   -->
  <p>自定义姓名：{{ store.getCustomName('中国') }}</p>
</template>


<script setup>
import { storeToRefs } from "pinia";
import { useUsersStore } from "./store/user"

const store = useUsersStore()
const { name } = storeToRefs(store)
</script>
```



## actions

### 创建

```js
import { defineStore } from 'pinia'

export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: '川普',
      age: 18,
      sex: '男',
      nationality: '中国'
    }
  },
  getters: {
  },
  actions: {
    // 异步展示
    async getName () {
      // const res = await xxx
    },
    // 同步展示
    saveName (name) {
      this.name = name;
    }
  }
})
```



### 使用

```vue
<script setup>
import { storeToRefs } from "pinia";
import { useUsersStore } from "./store/user"

const store = useUsersStore()

// 使用方法
store.getName()
</script>
```



# 数据持久化

还是与Vuex一样，刷新页面修改的数据会丢失，通过插件来取消重置。

## 安装

```shell
npm i  pinia-plugin-persistedstate
# 或者使用 yarn
yarn add pinia-plugin-persistedstate
```



## 全局挂载

main.js

```js
import { createPinia } from "pinia"
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)

const app = createApp(App)
app.use(pinia)
```



## 使用

```js
import { defineStore } from 'pinia'

export const useUsersStore = defineStore("users", {
  // 是否开启数据持久化
  persist: true,
  state: () => {
    return {
    }
  },
  getters: {
  },
  actions: {
  }
})
```

