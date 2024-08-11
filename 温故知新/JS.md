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

> 因为被定义在 `Date` 类本身上，而不是在 `Date` 实例上，所以不需要`new`。

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

就是`new`一下，没啥好说的。



# reduce

`reduce` 是 JavaScript 中数组对象的一个方法，它用于对数组中的每个元素执行一个回调函数，将其累积成单个值。`reduce` 常用于累加、计算总和、计算平均值、处理数据结构（如将数组转换为对象），甚至是实现类似于 `map` 和 `filter` 的功能。



## 基本语法

```js
array.reduce(callback(accumulator, currentValue, currentIndex, array), initialValue)
```

**callback**: 一个回调函数，在数组每个元素上执行，该函数包含四个参数：

- **accumulator**: 累加器，保存先前回调函数的返回值，或是 `initialValue`（如果提供了）。
- **currentValue**: 当前处理的数组元素。
- **currentIndex**: 当前处理元素的索引。如果提供了 `initialValue`，从 `0` 开始；否则从 `1` 开始。
- **array**: 调用 `reduce` 的数组。

**⭐️initialValue**（可选）: 作为第一次调用回调函数时 `accumulator` 的初始值。如果没有提供，`accumulator` 会使用数组的第一个元素，且回调函数从数组的第二个元素开始执行。



## 示例

- 求和

```js
const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
}, 0);

console.log(sum); // 输出 15
```

在这个例子中，`accumulator` 初始为 `0`，每次循环时将 `currentValue` 加到 `accumulator` 上，最终得到数组的总和 `15`。



- 数组转对象

```js
const people = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' }
];

const peopleById = people.reduce((accumulator, person) => {
    accumulator[person.id] = person;
    return accumulator;
}, {});

console.log(peopleById);
// 输出:
// {
//   1: { id: 1, name: 'Alice' },
//   2: { id: 2, name: 'Bob' },
//   3: { id: 3, name: 'Charlie' }
// }
```



- 计算数组中元素的频率

```js
const letters = ['a', 'b', 'a', 'c', 'b', 'a'];

const frequency = letters.reduce((accumulator, letter) => {
    accumulator[letter] = (accumulator[letter] || 0) + 1;
    return accumulator;
}, {});

console.log(frequency); 
// 输出: { a: 3, b: 2, c: 1 }
```

