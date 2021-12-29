# promise

```js
      new Promise(resolve => {
        resolve('hello')
        // reject('123') // 控制台会提示Uncaught (in promise) 123并停止往下执行
      }).then(val => {
        console.log(val) //  参数val = 'hello'
        return new Promise(resolve => {
          resolve('world')
        })
      }).then(val => {
        console.log(val) // 参数val = 'world'
      })
```

