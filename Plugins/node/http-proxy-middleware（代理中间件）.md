# http-proxy-middleware（代理中间件）

### install

``` shell
npm i http-proxy-middleware
```

### example

``` js
const http = require('http')
const { createProxyMiddleware } = require('http-proxy-middleware')

const server = http.createServer((req, res) => {
  // http://www.localhost:3000/api-ajax
  if (/\/api-ajax/.test(req.url)) {
    // 第一步
    const proxy = createProxyMiddleware('/api-ajax', {
      target: 'https://list.vip.com',
      changeOrigin: true,
    })
    // 第二步
    proxy(req, res)
  } else {
    console.log('error')
  }
})

server.listen(3000)

/* 
https://list.vip.com/api-ajax.php?callback=getMerchandiseIds&getPart=getMerchandiseRankList&fromIndex=0&mInfoNum=0&batchSize=0&r=101050987&q=%7C0%7C%7C0%7C0%7C1&brandStoreSns=&vipService=&props=&landingOption=&preview=0&sell_time_from=&time_from=&token=&_=1621487569659
 */
```