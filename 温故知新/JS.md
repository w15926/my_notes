[toc]

# 防抖

- 基础版本

```js
  const btn = document.querySelector('button')
  btn.addEventListener('click', () => {
    hanldeClick()
  })

  let timer = null
  const hanldeClick = () => {
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(() => {
      console.log('click')
    }, 1000)
  }
```

- 进阶版本

```js
  const btn = document.querySelector('button')
  btn.addEventListener('click', () => {
    hanldeClick()
  })

  const debounce = (fn, delay) => {
    let timer = null
    return () => {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(() => {
        fn()
      }, delay)
    }
  }

  const hanldeClick = debounce(() => {
    console.log('click')
  }, 1000)
```



# 节流

- 基础版本

```js
  const btn = document.querySelector('button')
  btn.addEventListener('click', () => {
    hanldeClick()
  })

  let lastCall = 0
  const hanldeClick = () => {
    const now = Date.now()
    if (now - lastCall < 1000) {
      return
    }
    lastCall = now
    console.log('click')
  }
```

- 进阶版本

```js
  const btn = document.querySelector('button')
  btn.addEventListener('click', () => {
    hanldeClick()
  })

  const throttle = (fn, delay) => {
    let lastCall = 0
    return () => {
      const now = Date.now()
      if (now - lastCall < delay) {
        return
      }
      lastCall = now
      fn()
    }
  }

  const hanldeClick = throttle(() => {
    console.log('Click')
  }, 1000)
```



# Date

## 静态方法

因为被定义在 `Date` 类本身上，而不是在 `Date` 实例上，所以不需要`new`。

- 返回当前时间的时间戳

````js
	console.log(Date.now())
````

- 解析字符串成时间戳

```js
  console.log(Date.parse('2020/01/01'))
  console.log(Date.parse('2020-01-01 01:23:45'))
  console.log(Date.parse('04 Dec 1995 00:12:00 GMT'))
```

- 返回UTC 时间的时间戳

```js
	// Date.UTC(year, month, day, hours, minutes, seconds, milliseconds)

	Date.UTC(2024, 7, 15, 12, 30, 45) // 1692103845000
	new Date(Date.UTC(2024, 7, 15, 12, 30, 45)) // Thu Aug 15 2024 20:30:45 GMT+0800 (中国标准时间)
```



## 实例方法

就是new一下，没啥好说的。
