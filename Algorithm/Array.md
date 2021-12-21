# 冒泡排序

```js
let arr = [-55, -2343, 55, 45643, 1, 4, 66]

for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length - 1; j++) {
    if (arr[j] > arr[j + 1]) {
      let temp = arr[j]
      arr[j] = arr[j + 1]
      arr[j + 1] = temp
    }
  }
}

console.log(arr)
```

