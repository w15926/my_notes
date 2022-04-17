# IOS

## 解除浏览器双击放大、两指放大

```js
// 双击放大
var lastTouchEnd = 0
document.documentElement.addEventListener('touchstart', event => {
  if (event.touches.length > 1) event.preventDefault()
}, false)

document.documentElement.addEventListener('touchend', event => {
  var now = Date.now()
  if (now - lastTouchEnd <= 300) event.preventDefault()
  lastTouchEnd = now
}, false)

// 两指放大
document.addEventListener('gesturestart', event => event.preventDefault())
```

