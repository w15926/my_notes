# vuex-persistedstate（页面刷新Vuex保存状态）

### install

```shell
npm i vuex-persistedstate
```

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