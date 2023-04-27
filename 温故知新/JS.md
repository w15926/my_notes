字符串转数组、对象

```js
var args = Array.prototype.slice.call(arguments);
var args = [].slice.call(arguments);

const args = Array.from(arguments);
const args = [...arguments];

作者：Always_positive
链接：https://juejin.cn/post/7041055543984652319
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



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

