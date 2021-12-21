# mime（根据后缀名返回对应的格式）

### install

``` shell
npm i mime
```

### example

``` js
// 根据请求地址自动匹配对应格式并返回数据

const http = require('http')
const fs = require('fs')
const path = require('path')

// 动态获取后缀名
const mime = require('mime') // 第三方插件根据对应的后缀名返回对应的格式

http.createServer((req, res) => {
  // 1.获取地址
  let pathName = req.url
  // 打开网页默认返回index.html
  pathName = pathName == '/' ? '/index.html' : pathName

  // 2.动态获取后缀名
  // path.extname可以获取后缀名
  const common = mime.getType(path.extname(pathName))

  // 3.通过fs模块读取文件
  if (pathName != '/favicon.ico') {
    fs.readFile(`./static${pathName}`, (err, data) => {
      if (err) {
        res.writeHead(404, { 'Content-Type': 'text/html;charset=utf-8' })
        res.end(`404，服务器找不到请求的网页`)
        console.log(err)
      }

      res.writeHead(200, { 'Content-Type': `${common};charset=utf-8` })
      res.end(data)
    })
  }

}).listen(3000)

console.log('http://127.0.0.1:3000/')
```