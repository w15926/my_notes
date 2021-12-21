![vuex](/Users/lalala/Library/Application Support/typora-user-images/Vue/Vuex/vuex.png)

# 结构

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: { // 相当于组件的 data
  },
  mutations: { // 相当于组件的 methods（同步）
  },
  actions: { // 相当于组件的 methods（异步）一般用于异步处理同步
  },
  getters:{ // 相当于组件的 computed
  },
  modules: { // 可引入多个vuex模块
  }
})
```



# state使用

### store/index.js

```js
  state: {
    name: '胖迪',
    age: 14
  },
```

### App.vue

``` js
<template>
  <div id="app">
    <!-- state使用 -->
    <h1>{{ $store.state.name }}</h1>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

```

### render page

![image-20210526101034828](/Users/lalala/Library/Application Support/typora-user-images/Vue/Vuex/image-20210526101034828.png)



# mutations使用

### store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    name: '胖迪',
    age: 14
  },
  mutations: {
    // state为vuex中的state，params为传递的自定义形参
    updateName(state, params) {
      state.name = '迪丽热巴'
    }
  }
})
```

### App.vue

```js
<template>
  <div id="app">
    <!-- state使用 -->
    <h1>{{ $store.state.name }}</h1>

    <!-- mutations使用 -->
    <button @click="updateName">mutations</button>
  </div>
</template>

<script>
// 方法二
import { mapMutations } from 'vuex'

export default {
  name: 'App',
  data() {
    return {
    }
  },
  methods: {
    updateName() {
      // this.$store.commit(参数一：调用的方法，参数二：传递的实参)
      // 比如this.$store.commit('namne',this.name)

      this.$store.commit('updateName') // 方法一
    },

    // 方法二
    // ...mapMutations(['updateName'])
  }
}
</script>

```

### render page

![dsfsdgrgr](/Users/lalala/Library/Application Support/typora-user-images/Vue/Vuex/dsfsdgrgr.gif)



# actions使用

### store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    name: '胖迪',
    age: 14
  },
  mutations: {
    updateName(state, params) {
      state.name = params
    }
  },
  actions: {
    // context为vuex中的store，params为传递的自定义形参
    asyncUpdateName(context, params) {
      context.commit('updateName', params)
    }
  }
})
```

### App.vue

``` js
<template>
  <div id="app2">
    <!-- state使用 -->
    <h1>{{ $store.state.name }}</h1>

    <!-- actions使用 -->
    <button @click="asyncUpdateName">actions</button>
  </div>
</template>

<script>
// 方法二
import { mapActions } from 'vuex'

export default {
  name: 'App2',
  data() {
    return {
      name: '热吧'
    }
  },
  methods: {
    asyncUpdateName() {
      this.$store.dispatch('asyncUpdateName', this.name) // 方法一
    },
    // ...mapActions(['asyncUpdateName']) // 方法二（不能传参）
  }
}
</script>
```

### render page

![tyisgiudsgvj](/Users/lalala/Library/Application Support/typora-user-images/Vue/Vuex/tyisgiudsgvj.gif)



# getters使用

### store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    name: '胖迪',
    age: 14
  },
  mutations: {
  },
  actions: {
  },
  getters: {
    // 计算属性用于复杂计算state（缓存）
    loadAge(state, params) {
      return state.age * state.age
    }
  },
  modules: {
  }
})
```

### App.vue

```js
<template>
  <div id="app3">
    <!-- getters使用 -->
    <h2>{{$store.getters.loadAge}}</h2>
  </div>
</template>

<script>
// 方法二
import { mapGetters } from 'vuex'

export default {
  name: 'App3',
  data() {
    return {
    }
  },
  methods: {
    loadAge() {
      return this.$store.getters.loadAge
    },
    // ...mapGetters(['loadAge'])
  }
}
</script>
```

### render page

![image-20210526131201720](/Users/lalala/Library/Application Support/typora-user-images/Vue/Vuex/image-20210526131201720.png)



# 页面刷新Vuex保存状态

### Vuex 3和 Vue 2

```js
import Vue from 'vue'
import Vuex from 'vuex'

// Vuex 3和 Vue 2 用法
import createPersistedState from 'vuex-persistedstate'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    name: '胖迪',
    age: 14
  },
  mutations: {
    change(state, params) {
      state.name = params
    }
  },
  actions: {
    asyncChange(context, params) {
      context.commit('change', params)
    }
  },
  getters: {
  },
  modules: {
  },
  plugins: [createPersistedState()] // 使用
  // plugins: [createPersistedState({ storage: window.sessionStorage })]
})
```

### Vuex 4和 Vue 3

```js
import Vue from 'vue'
// import Vuex from 'vuex'
import { createStore } from "vuex"

// Vuex 4和 Vue 4 用法
import createPersistedState from 'vuex-persistedstate'

Vue.use(Vuex)

export default createStore({
  state: {
    name: '胖迪',
    age: 14
  },
  mutations: {
    change(state, params) {
      state.name = params
    }
  },
  actions: {
    asyncChange(context, params) {
      context.commit('change', params)
    }
  },
  getters: {
  },
  modules: {
  },
  plugins: [createPersistedState()] // 使用
  // plugins: [createPersistedState({ storage: window.sessionStorage })]
})
```



# 封装（专业）

### 第一步：目录结构

> store
>
> > > modules
> > >
> > > > user.js
> > >
> > > getters.js
> > >
> > > index.js



### 第二步：./store/index.js

```js
import { createStore } from 'vuex'
import getters from './getters' // 调用modules文件夹里的所有文件（入口文件）
import createPersistedState from 'vuex-persistedstate'

// 自动引入.modules里的文件
const modulesFiles = require.context('./modules', true, /\.js$/);
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1')
  const value = modulesFiles(modulePath)
  modules[moduleName] = value.default
  return modules
}, {})

export default createStore({
  getters,
  modules,
  plugins: [createPersistedState()]
})
```



### 第三步：./store/modules/user

```js
const state = {
  currentSongUrl: '123'
}

const mutations = {

}

const actions = {

}

export default {
  namespaced: true, // 解决命名冲突
  state,
  mutations,
  actions
}
```



### 第四步：./store/getters

```js
// getters等价于computed计算属性
const getters = {
  currentSongUrl: state => state.user.currentSongUrl, // 计算属性
}

export default getters
```



### 第五步：使用

**讲解的是Vue3，Vue2加this就可以了**

```js
import { useStore } from 'vuex'

export default {
  setup() {
    const store = useStore()
    let name = computed(() => store.state.user.name) // 这里注意指定user模块
    
    // 得到./modules/user.js里state对象下的currentSongUrl属性
    console.log(store.state.user.currentSongUrl)
    
     store.commit('user/xxxx'） // 注意每个地方都要指定模块
                  
     // 使用getters
     store.getters.doneTodos 

    return {  }
  }
}
```



### 拓展

**快速重置state**

```js
// 声明函数为state值内容
const getDefaultState = () => {
  return {
    token: getToken(),
    name: '',
    avatar: '',
    userInfo: ''
  }
}

// 再赋值给state
const state = getDefaultState()

const mutations = {
  RESET_STATE: (state) => {
    // 利用Object.assign（深拷贝）对state进行重新赋值
    Object.assign(state, getDefaultState())
  },
```

