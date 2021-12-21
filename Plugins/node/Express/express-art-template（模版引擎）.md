# express-art-template（模版引擎）

### Install

````shell
npm install --save art-template
````

```shell
npm install --save express-art-template
```

### Example

#### js

```javascript
const express = require('express')

const app = express()

// express-art-template模版依赖art-template需要一起下载
// app.engine('art', require('express-art-template'))
app.engine('html', require('express-art-template'))

// 修改默认请求路径
app.set('views','./a')

app.get('/404', (req, res) => {
  // 不修改默认请求路径会默认去views文件夹目录下去找
  res.render('404.html', {
    title: '页面找不到了'
  })
})

app.listen(3000)
```

#### html

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h1>404{{ title }}</h1>
</body>

</html>
```



