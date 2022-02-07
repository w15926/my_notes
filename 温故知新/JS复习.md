
> 本篇个人笔记适用于有基础人群复习，并不会记录所有，会略过很多常用知识。
>
> https://zh.javascript.info/



# JavaScript 基础知识

## 关于标签

`<script>`标签如果做直接写代码使用，则每次进入界面都会加载一次。如果做`src`引入使用，则只有第一次使用到会加载，并存入缓存中，其他页面想要相同的脚本就会从缓存中获取而不是下载它。所以文件实际上只会下载一次。

> 如果使用了`src`后还在标签里写代码，则默认不会生效



## 交互

关于`alert`、`prompt`、`confirm`，这些方法都是模态的：它们***暂停脚本的执行***，并且不允许用户与该页面的其余部分进行交互，直到窗口被解除。




## 数据类型
### 关于typeof
>  **typeof null 的结果是 "object"。这是官方承认的 typeof 的行为上的错误，这个问题来自于 JavaScript 语言的早期，并为了兼容性而保留了下来。null 绝对不是一个 object。null 有自己的类型，它是一个特殊值。**



### 关基础运算符，数学

#### 数字型转换

> 在算术函数和表达式中，会自动进行 number 类型转换。
>
> 比如，当把除法 `/` 用于非 number 类型：

```js
alert( "6" / "2" ) // 3, string 类型的值被自动转换成 number 类型后进行计算
```



#### 数字转化，一元运算符+

> 一元运算符加号，或者说，加号 `+` 应用于单个值，对**数字**没有任何作用。但是如果运算元**不是数字**，加号 `+` 则会将其转化为数字。

一元运算符优先级（17）大于二元运算符优先级（13），所以会先执行一元运算再执行二元运算（JS中所有代码均按照优先级顺序执行）。

```js
// 对数字无效
let x = 1;
alert( +x ) // 1

let y = -2;
alert( +y ) // -2

// 转化非数字
alert( +true ) // 1
alert( +"" )   // 0

let str = '1'
const temp = +str
console.log(typeof temp, temp) // => number 1

// 等价操作
console.log(+'1' + +'2')
console.log(Number('1') + Number('2'))
```



### 自增/自减的报错

自增/自减**只能**应用于变量

```js
let i = 1
i++
console.log(i) // => 2

console.log(1++) // 报错
```



### 逗号运算符

它会被用来写更简短的代码

逗号运算符能让我们处理多个语句，使用 `,` 将它们分开。**每个语句都运行了**，但是只有**最后的语句的结果会被返回**。

> 逗号运算符的优先级特别低，比`=`还要低，因为常用于写在`()`内以提高优先级，否则可能会出现不执行或者报错

```js
let res_1 = 1
let res_2 = 2

// 逗号运算符里每个语句都会执行，但只返回最后的语句的结果
let res_3 = (res_1 = 2, res_1 + res_2)

console.log(res_1) // 2 在逗号运算符里被执行了
console.log(res_3) // 4 逗号运算符返回最后的语句的结果

// 逗号运算符的其他运用场景示例
res_1 > 1 ? (console.log('hello'), console.log('my'), console.log('girls')) : res_1

// 注意：在console输出各种日志中，这里的逗号仅代表显示多个值
console.log(1, 2) // 1 2
```



## 值的比较

### 字符串比较

在比较字符串的大小时，JavaScript 会使用“字典（dictionary）”或“词典（lexicographical）”顺序进行判定。

换言之，字符串是按字符（母）逐个进行比较的。

```js
// 如果前面都一样，会一个个往后匹配
console.log('abcd' > 'abc') // true

// 如果第一个字母直接大于第二个字母的第一个值，那么直接结束匹配
console.log('e' > 'abc') // true

// JS采用 unicode 编码顺序，在编码表索引中，字母 a 的 index 大于 A
console.log('a' > 'A')
```

 

一个神奇的现象

 - 若直接比较两个值，其结果是相等的。
 - 若把两个值转为布尔值，它们可能得出完全相反的结果，即一个是 `true`，一个是 `false`。

```js
let a = 0
console.log(Boolean(a)) // false

let b = "0"
console.log(Boolean(b)) // true

console.log(a == b) // true
```

> 对于 JavaScript 而言，这种现象其实挺正常的。因为 JavaScript 会把待比较的值**转化为数字后再做比较**（因此 `"0"` 变成了 `0`）。若只是将一个变量转化为 `Boolean` 值，则会使用**其他的类型转换**规则。



### 不同类型间的比较

- 使用 == 进行匹配比较会转换类型，以达到值相等类型可以不相等的要求

- 使用全等时会直接先比较类型

```js
console.log('2' > 1) // true，字符串 '2' 会被转化为数字 2
console.log('01' == 1) // true，字符串 '01' 会被转化为数字 1
console.log('01' === 1) // false

console.log('' == false) // true

// 对于布尔类型值，true 会被转化为 1、false 转化为 0
console.log(true == 1) // true
console.log(true === 1) // false
console.log(false == 0) // true
console.log(false === 0) // false
```



### 对 null 和 undefined 进行比较

当使用数学式或其他比较方法` < > <= >=` 时，`null/undefined `会被转化为数字：`null` 被转化为 0，`undefined` 被转化为 `NaN`。

```js
console.log(null == undefined) // true
console.log(null === undefined) // false
```



### null vs 0

在最后一行代码显示“`null` 大于等于 0”的情况下，前两行代码中一定会有一个是正确的，然而事实表明它们的结果都是 false。

因为相等性检查 `==` 和普通比较符 `> < >= <=` 的代码逻辑是相互独立的。进行值的比较时，`null` 会被转化为数字，因此它被转化为了 `0`。这就是为什么（3）中 `null >= 0` 返回值是 true，（1）中 `null > 0` 返回值是 false。

```js
console.log(null > 0) // false
console.log(null == 0) // false
console.log(null >= 0) // true
```



### 独立的undefined

`undefined` 不应该被与其他值进行比较

- `(1)` 和 `(2)` 都返回 `false` 是因为 `undefined` 在比较中被转换为了 `NaN`，而 `NaN` 是一个特殊的数值型值，它与任何值进行比较都会返回 `false`。
- `(3)` 返回 `false` 是因为这是一个相等性检查，而 `undefined` 只与 `null` 相等，不会与其他值相等。

```js
console.log(undefined > 0) // false
console.log(undefined < 0) // false
console.log(undefined == 0) // false
console.log(undefined === 0) // false
```



## 逻辑运算符

### ||逻辑或

- 类型转换

```js
if (0 || 1) { // 0经过类型转换成fasle， 1成true
  if (0) console.log('0')
  if (1) console.log('1') // print '1'
}
```



- 多结果为`true`时只返回**第一个**

```js
const value1 = 'value1', value2 = 'value2', value3 = 'value3'
const result = value1 || value2 || value3
console.log(result) // print -> value1
```



- 结果全为`false`时，返回**最后一个**

```js
const value1 = 0, value2 = false, value3 = null
const result = value1 || value2 || value3
console.log(result) // print -> null
```



- 短路求值

> 左侧的条件为假时才执行右侧侧命令，

```js
const variable = true
variable || console.log('hello world')
```



### &&逻辑或

- 多结果为`true`时只返回**最后一个**

```js
const value1 = 1, value2 = true, value3 = 'hello world'
const result = value1 && value2 && value3
console.log(result) // print -> hello world
```



- 多结果从左到右运算到第假值时，返回当前假值并且停止运行

```js
const value1 = 1, value2 = 0, value3 = 'hello world'
const result = value1 && value2 && value3
console.log(result) // print -> 0
```



- 优先级

> 与运算 `&&` 优先级大于或运算 `||` 

```js
 // 代码 a && b || c && d 跟 && 表达式加了括号完全一样：(a && b) || (c && d)
```



## ??空值合并运算符

- 当变量同时不为`null`和`undefined`时生效

```js
let a
const b = 'b'
const result = a ?? b// 等价于下一行代码
// const result = (a !== null && a !== undefined) ? a : b
console.log(result) // 'b
```



- 使用场景 - 为未定义的变量赋默认值

```js
let a
console.log(a) // undefined
a = a ?? 'a'
console.log(a) // 'a'
```



- 进阶使用

>  使用 `??` 序列从一系列的值中选择出第一个非 `null/undefined` 的值

```js
let a = null
let b = 0
let c = 'c'

// 显示第一个已定义的值：
console.log(a ?? b ?? c ?? 'd') // 0

// 与 三元运算符 比较
// 区别： 三元运算符 返回第一个真值，??返回第一个已定义
console.log(a ? a : b ? b : c ? c : 'd' ? 'd' : 'd') // c

// 与 || 比较
// 区别： || 返回第一个真值，??返回第一个已定义
console.log(a || b || c || 'd') // c
```



- 优先级

> 禁止将 `??` 运算符与 `&&` 和 `||` 运算符一起使用，除非使用括号明确指定了优先级

```js
// () > * > || > ??

let x = 1 && 2 ?? 3
console.log(x) // 报错

let y = (1 && 2) ?? 3
console.log(y) // 2
```



## 关于循环

### for

- break的指定退出

> 如果不使用`break outer`直接写`break`就会退出子循环，而继续执行父循环的代码

```js
outer: for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = 2

    if (input === 2) break outer // 直接退出上一级的循环

  }
  console.log(1) // 最外级循环被break，这行不会执行
}
```



### switch

> switch循环的`case`匹配是采用`===`方式



- 全等`case`的类型判断

> `case`是执行`===`进行匹配，但是当写了`||`这种具有类型判断的时候，会先进行类型转化判断，转化后再进行全等判断

```js
let variable = 0

// 正常执行
switch (variable) {
  case 0:
    console.log(0)
}

// 不会执行
switch (variable) {
  case 0 || '0':
    console.log('fail')
}

// 不会执行
switch (variable) {
  case '0' || 0:
    console.log('fail')
}
```



- 无`break`的问题

> 代码会在`case`匹配成功后继续往下执行，直到运行到break或最后

```js
let variable = 1

switch (variable) {
  case 0:
    console.log(0)
  case 1:
    console.log(1) // 执行
  case 2:
    console.log(2) // 执行
  default:
    console.log('default') // 执行
}
```



#### case分组

如果想让两个`case`执行同一个方法，可以按照以下写法（实际还是利用**无break的问题**）

```js
let variable = 1

// 如果让case2也执行case1的的代码
switch (variable) {
  case 0:
    console.log(0)
    break
  case 1:
  case 2:
    console.log(2) // 执行
    break
  default:
    console.log('default') // 所以case不匹配时才执行
}
```



## 关于函数

- 修改形参实参不会更改，因为此时的形参相当于一个副本

```js
let uname = 'pd'

function showName(uname){
  uname += '胖迪'
  console.log(uname) // pd胖迪
}
showName(uname)

console.log(uname) // pd
```



### 形参默认值

形参没有接受到值时存在`undefined`默认值

```js
// 初始默认值
function showName(uname, empty){
  console.log(uname) // pd
  console.log(empty) // undefined
}
showName('pd')




// 手动设置默认值 方式一 - 直接设置形参：
function showName (uname = '胖迪', empty = 'no text given') {
  console.log(uname) // pd
  console.log(empty) // no text given
}
showName('pd')




// 手动设置默认值 方式二 - 后置修改：
function showName (uname) {
  // Method 1
  // if(!uname){ // 使用 == 比较，存在类型转换。
  //   uname = 'Default argument'
  //   // Do something...
  // }

  // Method 2
  uname = uname ?? 'Default argument' // 推荐使用空值运算符，不存在类型转换。

  // Method 3
  // uname = uname || 'Default argument' // 使用 == 比较，存在类型转换。
  
  console.log(uname)
}
showName(0)
```



### 默认返回值

如果`return`为空，则默认返回`undefined`

```js
const doNothing = () => {}
// function doNothing(){
//   return
// }
console.log(doNothing() === undefined) // true
```



### 函数表达式与函数声明的调用区别

- 函数声明

```js
// 正常执行
function sayHi(name) {
  console.log( `Hello, ${name}` )
}

sayHi('pd')

// 正常执行
sayHi('pd')

function sayHi(name) {
  console.log( `Hello, ${name}` )
}
```

- 函数表达式

```js
// 失败，变量未赋值
sayHi('pd') // Cannot access 'sayHi' before initialization

let sayHi = function(name) {
  console.log( `Hello, ${name}` )
}

// 正常执行
let sayHi = function(name) {
  console.log( `Hello, ${name}` )
}

sayHi('pd')
```



# Object基础知识

## 对象

### delete操作符

> 用于删除某一key

```js
let obj = {
  uname: 'pd',
  age: 18,
  gender: 0
}

delete obj.uname

console.log(obj) // { age: 18, gender: 0 }
```



### in操作符

> 用于检测某一key是否存在

```js
let obj = {
  uname: undefined
}

// 此处如果用是否等于 undefined 来判断的话会出错
console.log('uname' in obj) // true
```



### 方括号`[]`

> 基础使用

```js
let girl = {
  'She is a beautiful girl': true
}
console.log(girl['She is a beautiful girl']) // true

```



> 计算属性

```js
let fruit = 'banana'

let bag = {
  [fruit]: 7 // 此时fruit为计算属性，key为fruit变量的value
}

console.log(bag.banana) // 7
console.log(bag.orange) // undefined

// 计算属性的复杂写法
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5
}
console.log(bag) // { appleComputers: 5 
```



> 计算属性的另一种写法

```js
let fruit = 'tomato'
let bag = {}

// key = fruit 变量的 value
bag[fruit] = 5

console.log(bag) // { tomato: 5 }
```



### 属性名称限制

> 属性命名没有限制。属性名可以是任何字符串或者 symbol，其他类型会被自动地转换为字符串。

```js
let obj = {
  0: "test" // 等同于 "0": "test"
}

// 都会输出相同的属性（数字 0 被转为字符串 "0"）
console.log(obj['0']) // test
console.log(obj[0]) // test
console.log(obj) // { '0': 'test' }
```

