# mock.js（模拟数据）

通过拦截XHR返回数据，所以浏览器并没有真正发送请求

### install

```shell
npm i mockjs
```

### 常用语法

```js
		// http://mockjs.com/examples.html#

    "id|1-9999": 0, // 返回 1 到 9999 随机数
    "username": "@cname", // @cname为默认随机中文名  @name为随机英文名
    "age|1-100": 0,
    "email": "@email", // 随机邮箱

    "city":'@province', // 随机 省份
    // "city":"@city", // 随机 城市
    // "city": "@county", // 随机 县
    // "city":"@city(true)", // 随机 "省份 城市"  例如 "江苏 连云港"
    // "city": "@county(true)", // 随机 "省份 城市 县"  例如 "福建省 南平市 光泽县"

    'date': '@datetime()', //随机时间 例如 "1982-12-03 23:27:38"
    'img': '@image()',//随机图片 例如 "http://dummyimage.com/120x60"
    'price': '@integer(1,100)'//随机价格 返回 1 到 100 整数
    "message": '@cword(6)' // 随机汉字（6个） 去掉c为字母

		// --------------------------------------------------------------------

    "persons|100": [ // 该数组中返回 100 个下面定义的对象
      {
        "id|+1": 0, // 从 0 开始每次 +1    012345
        ...
      }
    ]
```

### main.js

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import './mock' // 导入 mock 所在路径

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```

### get

#### 正常请求

```js
    get() {
      axios.get('/user/login')
        .then(res => console.log(res))
        .catch(err => console.log(err))
    }
```

/src/mock/index.js

```js
import Mock from 'mockjs'

// get  默认请求方法为get，第二个参数可不写  Mock.mock( rurl, rtype, template )
Mock.mock('/user/login','get', {
  "status": 200,
  "data": {
    "id|1-9999": 0,
    "username": "@cname",
    "age|1-100": 0,
    "email": "@email",
  }
})
```

#### 携带params请求

```js
    get() {
      axios.get('/user/login', {
        params: {
          id: 1
        }
      }
      )
        .then(res => console.log(res))
        .catch(err => console.log(err))
    }
```

/src/mock/index.js

```js
// get + params  例如 /user/login?id=1  用正则过滤掉
Mock.mock(RegExp("/user/login" + ".*"), 'get', {
  "status": 200,
  "data": {
    "id|1-9999": 0,
    "username": "@cname",
    "age|1-100": 0,
    "email": "@email",
  }
})
```

### post

#### request

```js
    post() {
      axios.post('/products/params', {
        id: 0,
        price: 4399
      })
        .then(res => console.log('res响应成功',res))
        .catch(err => console.log(err))
    }
```

#### mock

```js
// post
Mock.mock('/products/params', 'post', function (options) {
  console.log('mock接收到到参数为：', options)
  if (options.body === '{"id":0,"price":4399}') {
    return {
      status: 200,
      products: 'iphone4S'
    }
  }
})
```

### 增

#### request

```js
      axios.post('/params/list', {
        product: 'iphone5',
        id: 4
      })
        .then(res => console.log(res))
        .catch(err => console.log(err))
```

#### mock

```js
const paramsList = [
  { product: 'iphone3G', id: 1 },
  { product: 'iphone4', id: 2 },
  { product: 'iphone4S', id: 3 },
]

Mock.mock('/params/list', 'post', options => {
  let body = JSON.parse(options.body) // 获取请求参数
  let id = parseInt(body.id)
  let flag = true

  for (let item of paramsList) {
    if (item.id === id) flag = false // 判断id是否已经存在
  }

  // 如果id不存在
  if (flag) {
    paramsList.push(
      {
        product: body.product,
        id
      }
    )
    return {
      paramsList,
      status: 200,
      msg: '添加成功'
    }
  }
  // 如果id已存在
  return {
    status: 400,
    msg: '添加失败，id已存在'
  }

})
```

### 删

#### request

```js
axios.delete('/params/list', {
        // data等价于post方式
        data: {
          id: 2
        }

        // params等价于get方式，url可见
        // params: {
        //   id: 2
        // }
      })
        .then(res => console.log(res))
        .catch(err => console.log(err))
```

#### mock

```js
const paramsList = [
  { product: 'iphone3G', id: 1 },
  { product: 'iphone4', id: 2 },
  { product: 'iphone4S', id: 3 },
]

Mock.mock('/params/list', 'delete', options => {
  let id = parseInt(JSON.parse(options.body).id) // 获取请求id
  let index = null

  for (let i in paramsList) {
    if (paramsList[i].id === id) index = i // 获取id对应的index
  }

  if (index !== null) {
    console.log(`删除的id为：${id}，对应的index为：${index}`)
    paramsList.splice(index, 1) // 从index开始删除一个（本身）
    return {
      paramsList,
      status: 200,
      msg: '删除成功'
    }
  }
  return {
    status: 400,
    msg: '删除失败，id不存在'
  }

})
```

### 改

#### request

```js
      // 语意化可以使用put和dispatch,或者全部post都可以
      axios.post('/params/list', {
        function: {
          type: 'modify',
          data: {
            id: 1, // 要修改的id
            product: 'iphone' // 修改的内容
          }
        }
      })
        .then(res => console.log(res))
        .catch(err => console.log(err))
```

#### mock

```js
const paramsList = [
  { product: 'iphone3G', id: 1 },
  { product: 'iphone4', id: 2 },
  { product: 'iphone4S', id: 3 },
]

Mock.mock('/params/list', 'post', options => {
  const parseBody = JSON.parse(options.body) // 解析请求参数（只能解析到这）
  const body = parseBody.function.data // 获取请求参数中的data
  const id = parseInt(body.id) // 获取请求id
  let flag = true
  let index = null

  if (parseBody.function.type === 'modify') {

    for (let i in paramsList) {
      if (paramsList[i].id === id) { // 判断id是否已经存在
        flag = false, index = i // 拿到id对应的index
      }
    }

    // 如果id不存在
    if (flag) {
      return {
        status: 400,
        msg: 'id不存在'
      }
    }
    // 如果id存在
    paramsList[index].product = body.product
    return {
      paramsList,
      status: 200,
      msg: '修改成功'
    }

  } else {
    return {
      status: 400,
      msg: '请求类型错误'
    }
  }

})
```

### 查

#### request

```js
      axios.post('/params/list', {
        function: {
          type: 'lookup',
          data: {
            id: 2, // 根据id查找对应的信息
          }
        }
      })
        .then(res => console.log(res))
        .catch(err => console.log(err))
```

#### mock

```js
const paramsList = [
  { product: 'iphone3G', id: 1 },
  { product: 'iphone4', id: 2 },
  { product: 'iphone4S', id: 3 },
]

// 查 （此处为查询某一条，批量查询：遍历id拿到对应的index并push进数组里，
//     返回时循环paramsList[index] push进一个数组里，返回数组）
Mock.mock('/params/list', 'post', options => {
  const parseBody = JSON.parse(options.body) // 解析请求参数（只能解析到这）
  const body = parseBody.function.data // 获取请求参数中的data
  const id = parseInt(body.id) // 获取请求id
  let flag = true
  let index = null

  if (parseBody.function.type === 'lookup') {

    for (let i in paramsList) {
      if (paramsList[i].id === id) { // 判断id是否已经存在
        flag = false, index = i // 拿到id对应的index
      }
    }

    // 如果id不存在
    if (flag) {
      return {
        status: 400,
        msg: 'id不存在'
      }
    }
    // 如果id存在
    let searchParams = paramsList[index]
    return {
      searchParams,
      status: 200,
      msg: '查询成功'
    }

  } else {
    return {
      status: 400,
      msg: '请求类型错误'
    }
  }
})
```
