# 入口

``` js
const express = require('express')
const app = express()

// 已弃用（解析post）
// const bodyParser = require('body-parser')

const router = require('./router/index')



// 已弃用
// app.use(bodyParser.urlencoded({ extended: false }))

// 新版直接内置了，使用express自带的就行（一定要写在路由前面）
app.use(express.json()) // 解析JSON字符串
app.use(express.urlencoded({ extended: true })) // 解析表单



app.use('/', router) // 加载路由

app.listen(3000, _ => console.log('start successfully...'))

/*
  建议使用app.use(express.json())
    app.use(express.urlencoded({ extended: true }))
 */
```



# 路由

### 结构

``` js
const express = require('express')
const router = express.Router()

router.get('/', (req, res, next) => res.send('hello'))

router.get('/index', (req, res, next) => {

})

router.post('/index', (req, res, next) => {

})

module.exports = router
```



### get

``` js
// get methods
// http://localhost:3000/index?a=1
router.get('/index', (req, res, next) => {
  const query = req.query
  // 方法一    默认返回JSON格式
  res.send(query)

  // 方法二    手动转成JSON
  // res.json(query)
})
```



### post

``` js
// post methods
router.post('/index', (req, res, next) => {
  console.log(req.body)
  res.send(req.body)
})
```



### other

``` js
// 来者不拒
router.all('/index', (req, res, next) => { })

// put 修改数据（覆盖）
router.put('/index', (req, res, next) => {
  console.log(req.body)
  res.send(req.body)
})

// patch 修改数据（选择性）
router.patch('/index', (req, res, next) => { })

// delete 删除数据
router.delete('/index', (req, res, next) => { })
```

