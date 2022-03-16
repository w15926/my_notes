# main.js

除了基础的配置，还按需引用了常用的element-ui库，并且初始化了全局CSS与filter和事件总线。

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

import * as echarts from 'echarts'
import './utils/el'
import './utils/filter'
import 'normalize.css/normalize.css'

Vue.config.productionTip = false
Vue.prototype.$echarts = echarts
Vue.prototype.$bus = new Vue()

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```



# 高德地图与openLayers地图

目录

> src
>
> > views
> >
> > > Demo
> > >
> > > > index.vue

配置了两款地图，并且做了初始化的演示，项目运行可直接查看效果。

```vue
    <!-- 演示openlayers （采用模块化开发方案） -->
    <olMap />
    <!-- 演示高德地图 -->
    <div id="map_Gaode" />
```



# Cookie配置token封装

目录

> src
>
> > utils
> >
> > > auth

在任意界面可以直接按需导入其中的某个方法，也可以自己继续增加，项目中操作cookie时统一在这添加，方便统一管理。

注意：次auth文件只存放token管理。

```js
import Cookies from "js-cookie";

// 用户登录 token
const userTokenKey = 'location_user_token'
export const getUserToken = () => Cookies.get(userTokenKey)
export const setUserToken = token => Cookies.set(userTokenKey, token)
export const removeUserToken = () => Cookies.remove(userTokenKey)

// 接口认证 token 过期时间
const interfaceTokenExpire = 'location_interface_token_expire'
export const getTokenExpire = () => Cookies.get(interfaceTokenExpire)
export const setTokenExpire = token => Cookies.set(interfaceTokenExpire, token)
export const removeTokenExpire = () => Cookies.remove(interfaceTokenExpire)

// 接口认证 token 值
const interfaceTokenVal = 'location_interface_token_val'
export const getTokenVal = () => Cookies.get(interfaceTokenVal)
export const setTokenVal = token => Cookies.set(interfaceTokenVal, token)
export const removeTokenVal = () => Cookies.remove(interfaceTokenVal)
```



# APId调用统一管理

## 配置全局BASE_URL

目录

> public
>
> > config.js

```js
const BASE_URL ='api/lightning_protection'

window.g = {
  AXIOS_TIMEOUT: 10000,
  BASE_API: `${BASE_URL}`,
  BASE_SOCKETPATH: ``
}
```

> 注意上诉代码`BASE_URL`中使用了`api`，其目的为了做前端跨域做处理。



## 引用

在**index.html**文件里使用`script`标签引用即可

```js
<script src="./config.js" />
```



## 前端跨域处理

vue.config.js

```js
module.exports = {
  devServer: {
    host: 'localhost',
    port,
    open: false,
    proxy: {
      '/api': {
        // target: 'http://192.168.3.169:8083',
        // target: 'http://www.mw3tech.com',
        target: 'https://www.hainanqx.cn',
        changeOrigin: true,
        pathRewrite: {
          '/api': ''
        }
      }
    }
  }
}
```

> 配置全局BASE_URL的时候使用了`api`，其目的是访问接口的时候自动将`api`转换成上诉代码中`target`路径。



## 拦截器封装

目录

> src
>
> > utils
> >
> > > request.js

此拦截器中使用了接口认证功能，在访问接口前需调用另一个接口获取token，再拿着这token进行你的接口数据请求。

并且支持了`new FormData()`操作，此拦截器不可随意改动，如需改动需要与技术组、主管进行沟通。

```js
/**
 * @auth 王乐翔
 * @param {String} title 接口拦截器
 * @Description 包含接口认证token和用户登录token的认证
 * @returns interface data
 */

import axios from 'axios'
import {
  getTokenExpire,
  setTokenExpire,
  getTokenVal,
  setTokenVal
} from '@/utils/auth'

export const myRequest = opt => {

  let isExpire = true

  if (getTokenExpire()) {
    let expireTimeStamp = getTokenExpire()
    expireTimeStamp - new Date().getTime() <= 0 ? isExpire = true : isExpire = false
  }

  return new Promise((resolve, reject) => {

    if (isExpire) {
      // 请求token
      axios.post(`${window.g.BASE_API}/api/login`, {
        //  mobile: 'MWS',
        //  password: 'MWS123456'
        mobile: 'admin',
        password: '123456'
      })
        .then(res => {
          if (res.status != 200 || !res.data || res.data.code != 0) return
          let extimes = new Date().getTime() + res.data.expire // 保持时间（毫秒数）
          setTokenExpire(extimes)
          isExpire = false
          setTokenVal(res.data.token)
          // 请求接口数据
          resolve(requestData(res.data.token)
            .then(res => res.data)
            .catch(err => err)) // 如果后端统一错误返回，则可在这直接打印在代码中就不需要使用try catch
        })
        .catch(err => Promise.reject(new Error('获取接口认证token失败', err)))
    }
    // 请求接口数据
    else resolve(requestData(getTokenVal())
      .then(res => res.data)
      .catch(err => err))
  })

  // 拿到token请求接口
  function requestData (token) {
    if (opt.method === 'get' || opt.method === 'GET') {
      return axios.get(`${window.g.BASE_API}/${opt.url}`,
        {
          params: opt.params,
          headers: { token }
        }
      )
    }
    if (opt.method === 'post' || opt.method === 'POST') {
      let data = null
      if (opt.isFormData) {
        data = new FormData()
        Object.keys(opt.params).forEach(key => data.append(key, opt.params[key]))
      }
      else data = opt.params

      return axios.post(`${window.g.BASE_API}/${opt.url}`,
        data,
        {
          headers: { token }
        }
      )
    }
  }

}
```



## api调用统一管理

目录

> src
>
> > api
> >
> > > demo.js（此文件名应与接口同步）

```js
/**
 * @auth 王乐翔
 * @param {String} title demo界面相关接口
 * @returns interface data
 */

import { myRequest } from '@/utils/request'

// 演示封装调用接口，做统一管理    方法名应与接口同步
export const getDemoData = params =>
  myRequest({ url: 'api/userLogin', method: 'post', params, isFormData: true, g1 }) // 需要使用FormData时加上，g1是调用public/config.js里的第二套接口
```

示意

```js
/* 预警统计（此处是接口文档大标题） */
import { myRequest } from '@/utils/request'

// 各城市预警类型次数（此处也是取自接口说明标题）
export const statisticsCountAlertMultiCity = params => // 这个变量名也是取自接口，方便统一管理
  myRequest({ url: '/alarm/statisticsCountAlertMultiCity', params })
```



## 调用接口

目录

> src
>
> > views
> >
> > > Demo
> > >
> > > > index.vue

按需解构引用

```js
import { getDemoData } from '@/api/demo'
```

使用

```js
async getDemoData () {
      try {
        // 演示一：表单提交
        // let data = new FormData()
        // data.append('loginData', JSON.stringify({ username: 'test', password: 'test' }))
        // const { ds: res } = await getDemoData(data)

        // 演示二：表单提交
        // let data = new FormData()
        // data.append('userName', 'admin659237')
        // data.append('password', '659237prG+-')
        // const { ds: res } = await getDemoData(data)

        // 演示三：统一封装，如果使用表单提交则在api里params后加上isFormData：true
        const data = { userName: 'admin659237', password: '659237prG+-' }
        const res = await getDemoData(data)

        console.log('接口数据', res)
      } catch (error) {
        console.error(error);
      }
    }
```

> 如果后端统一接口请求失败返回数据，则可以不使用`try catch`，在拦截器里配置处理即可（拦截器代码中有相应的注释）。



# Vuex

## 封装

目录

> src
>
> > store
> >
> > > modules
> > >
> > > > user.js
> > >
> > > getter.js
> > >
> > > index.js

由于刷新网页Vuex状态会销毁，所以对其进行了相关配置，到手即用。

在index.js中配置了自动引用modules文件夹下的所有文件与相关插件，所以此文件不要随意改动，如果改动需与技术组和主管沟通。

```js
import Vue from 'vue'
import Vuex from 'vuex'
import getters from './getters'
import createPersistedState from 'vuex-persistedstate'

Vue.use(Vuex)

// 自动引入.modules里的文件
const modulesFiles = require.context('./modules', true, /\.js$/);
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1')
  const value = modulesFiles(modulePath)
  modules[moduleName] = value.default
  return modules
}, {})

export default new Vuex.Store({
  getters,
  modules,
  plugins: [createPersistedState({
    storage: window.sessionStorage
  })]
})
```



user.js文件

```js
const state = {
  name: '胖迪'
}

const mutations = {
  changeName (state, params) {
    state.name = params
  }
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



## 使用

唯一区别就是调用规则发生细节变化，在`state`后面需要加上你的模块名（modules文件夹下的某个文件名）

```js
    this.$store.commit('user/changeName', '迪丽热巴') // vuex 演示，已处理刷新页面丢失数据问题
    console.log('Vuex内容：', this.$store.state.user.name)
```



# 生产与开发环境配置

vue.config.js

为了避免端口冲突等异常情况，采用9528端口进行开发。并且配置了生产环境的相对路径相关操作。

```js
'use strict'
const path = require('path')
const defaultSettings = require('./src/settings.js')

function resolve (dir) {
  return path.join(__dirname, dir)
}

const name = defaultSettings.title || ''

const port = process.env.port || process.env.npm_config_port || 9528

module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '././' : '/',
  outputDir: 'dist',
  assetsDir: 'static',
  lintOnSave: process.env.NODE_ENV === 'development',
  productionSourceMap: false,
  devServer: {
    host: 'localhost',
    port,
    open: false,
    proxy: {
      // ...
    }
  },
  configureWebpack: {
    name,
    resolve: {
      alias: {
        '@': resolve('src')
      }
    }
  },
}
```



# CSS的全局配置与mixin

目录

> src
>
> > assets
> >
> > > css
> > >
> > > > base.css
> > > >
> > > > mixin.scss

在base.css中初始化全局CSS 样式与全局CSS变量等相关操作

```css
/* 已用normalize初始化项目，这里写额外需要初始化的全局样式 */
* {
  box-sizing: border-box !important;
}

body {
  /* iOS Safari */
  -webkit-touch-callout: none;
  /* Chrome/Safari/Opera */
  -webkit-user-select: none;
  /* Konqueror */
  -khtml-user-select: none;
  /* Firefox */
  -moz-user-select: none;
  /* Internet Explorer/Edge */
  -ms-user-select: none;
  user-select: none;
}

ul, li {
  padding: 0;
  margin: 0;
  list-style: none
}
```

在mixin.scss中封装CSS函数

```css
// 文本溢出省略号隐藏
@mixin textEllipsis($lineCount:1) {
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  word-wrap: break-word;
  -webkit-line-clamp: $lineCount;
  -webkit-box-orient: vertical;
}
```



# Vue2的全局过滤器

目录

> src
>
> > utils
> >
> > > filter.js
> > >
> > > methods.js

不需要在main.js里配置，main.js里不需要太多不必要的代码，要尽量简洁。

所有的过滤器应该统一文件管理，在此文件配置项目所有需要的过滤器。

```js
import Vue from 'vue'

// 米转公里
Vue.filter('filterDistance', val => {
  if (!val || val === '') return
  const num = Number(val)
  let res = (num / 1000)
  if (res.toString().substring(0, 1) === '0') return res.toFixed(2) + 'km'
  return res + 'km'
})
```

通过管道符号使用

**注意：仅能在moustache中使用**

```vue
    <!-- 演示Vue2的全局过滤器 -->
    <div>{{31415926 | filterDistance}}</div>
```



# methods.js公用方法使用

目录

> src
>
> > utils
> >
> > > methods.js

此文件写入公用JS方法，在需要的文件里使用`import`按需导入。

```js
// 封装某某方法
export const say = str => 'say：' + str
```

使用

```js
import { say } from '@/utils/methods'
```

```js
console.log(say('胖迪')) // say：胖迪
```

