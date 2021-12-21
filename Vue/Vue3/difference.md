# router



## children

```js
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    name: 'home',
    component: () => import('@/views/Home.vue'),
    children: [
      // 在vue-router 4.0 中，children属性内不会自动加 / ，每个路径前需要手动加 /
      // 使用redirect来默认加载children里的某一条路径
      {
        path: '',
        redirect: '/add' // path
        // redirect: { name: 'add' } // name
      },
      {
        path: '/add', // 以前默认有 / 不需要写，现在必须写
        name: 'add',
        component: () => import('../components/Add'), // 默认寻找目录下index文件
        // component: () => import('@/components/Add/index'), // 使用@后不会再默认找
        meta: { title: '添加' }
      },
```



## 路由跳转传参

```js
import { useRouter, useRoute, useLink } from 'vue-router'
```

**把Vue2中的this.$router写成以下方式，其他一样**

```js
    const router = useRouter()
    const route = useRoute()
    const link = useLink()
```

