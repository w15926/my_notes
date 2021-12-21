等价Java里`Thread.sleep(1000)`

# Method 1

使用async 将异步方法变为同步方法执行，只有返回 resolve才会触发 await向下执行。

```js
const sleep = (timeout= 1000)=>new Promise((resolve, reject)=>{
    setTimeout(resolve, timeout);
});
// 可以使用 bluebird模块中的 bluebird.delay() 替换 sleep()
// const bluebird = ruquire('bluebird');
let timer = async(timeout) => {
    for(let i = 0; i< timeout; i++) {
        await sleep(1000);
        console.log(i+1);
    }
}
timer(10);
```



# Method 2

使用 setInterval（）方法实现

```js
function timer(timeout){
    let i = 0;
    let t;
    t = setInterval(time, 1000);
    function time() {
        console.log(i);
        i++;
        if(i >= timeout) clearInterval(t);
    }
}
timer(10);
```



# Method 3

使用 setTimeout 实现

```js
function timer(timeout) {
    let i = 0;
    // let t;
    time();
    function time() {
        if (i < timeout) {
            console.log(i);
            i++;
            setTimeout(time, 1000);
        } else {
            return;
        }
    }
}
timer(10);
```

