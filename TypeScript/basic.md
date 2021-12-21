# 环境

```shell
npm i -g typescript
```



# 编译

会转换成x x x.js，和javac一样转换成class后执行

```shell
tsc xxx.ts
```



## 全局变量提示

TS编译的时候会对变量进行全局声明，即”a“文件里声明变量`a`后再在“b”文件里声明变量`a`会报错提示变量已经声明

解决方式一：

```tsx
export {} // 取消 “无法重新声明块范围变量” 提示
```

原理：让tslint以为这是一个ESModule模块，因此不存在变量重复声明的可能性。



## 转换版本

TS默认转换为ES3，可以指定转换成任意ES版本。



# 类型

### basic

```typescript
let a: number
a = 1
a = 10
// a = "a" // 报错

let b: boolean = false

let c = false
// c = 123 // 报错，c声明过false，此时c的类型为boolean

function sum(a: number, b: number): number { // 最后的:number为函数返回值类型
  return a + b
}
sum(123, 456)
// sum(123, "456") // 报错

let d = 10
// d = '10' // 报错，具有类型推断功能，此时 d 已为 Number 型
```

```typescript
// 通过字面量声明赋值，不可修改值
let a: 10
a = 10 // 还是等于原来的值，编译通过
// a = 11 // 报错，不可以修改


// 使用 | 来连接多个类型（联合类型）
let b: 'hello' | 'world' | boolean
// console.log(b) // b不赋值默认为undefined
b = 'world' // b只能赋值声明的值
b = true
// b = 123 // 报错，没有声明过number类型，且值不为'hello'或者'world'


// any任意类型（关闭该变量的类型检测）
let c: any // 显式声明
c = true
c = 123
c = '456'

let c2 // 隐式声明any
c2 = true
c2 = 123

let c3: string = '123'
c3 = c // any可以直接赋值给其他类型
console.log(c3, typeof c3) // 456 string


// unknown未知类型（实际上就是安全的any）
let d: unknown
d = 123
d = 'false'

let d2: string = '123'
// d2 = d // 报错 不能将类型“unknown”直接分配给其他类型
if (typeof d === 'string') { // 需要判断才能赋值，TS里打印的类型为String格式
  d2 = d
  console.log(d2) // 'false'
}
d2 = d as string // 类型断言 和判断结果一样 让d作为string赋值给d2
d2 = <string>d // 类型断言方式二


// void 空
function fn(): void {
  // return null
  // return undefined
  return
}


// never 没有返回值
function fn2(): never {
  throw new Error('出错了')
}
```



### object

```typescript
// 开发不用声明成对象，因为万物皆对象
let a: object // a声明成一个对象
let a1: {} // 通过字面量生成对象
console.log(typeof a1)

// 开发常用
// 声明为对象并规定只能有一个string类型的name值
let b: { name: string }
b = { name: 'pd' }
// b = { name: 'dlrb', sex: 'woman' } // 报错，只能使用指定的name

// 声明一个name，并增加一个可有可无的age属性
let b2: { name: string, age?: number }
b2 = { name: '胖迪' }
b2 = { name: '胖迪', age: 18 }

// 声明一个name并且后面可以无限加属性
let b3: { name: string, [propName: string]: unknown } // 因为JS的属性名实际上就是字符串，所以propname: string
b3 = { name: 'pd', age: 18, sex: 'woman' } // 必须包含name，其他值可以随便加
// b3 = { age: 18 } // 报错，必须加name
```



### function

```typescript
// 声明fn为函数，函数内有a、b两个值，并且函数返回number型
let fn: (a: number, b: number) => number
fn = function (n1, n2) { // 此时n1、n2的类型为number
  return n1 + n2 // 函数的返回结果也要是number
}
// fn = function (n1: string, n2) { // 报错 n1已指定是number，不能使用string
//   return n1 + n2 // 报错 返回值是string不是number
// }
```



### array

```typescript
// 声明只能存放string类型的数组，以此类推
let arr: string[]
arr = ['a', 'b']

// 方式二
let arr2: Array<string>
arr2 = ['a', 'b']

let arr3: Array<unknown>
arr3 = [1, "2", new Date()]
```



### tuple

```typescript
// tuple 元祖（长度固定的数组）
let arr4: [string, string, number]
arr4 = ['1', '2', 3] // 顺序不可乱
```



### enum

```typescript
// enumeration 枚举
// let enum: { name: string } // 报错 不能使用enum关键字

enum Gender {
  woman, // 默认 0
  man, // 默认 1 （以此类推）
  other = '?' // 可以手动修改
}

// let e: { name: string, gender: 0 | 1 }
// e = {
//   name: '胖迪',
//   gender: 0
// }

let e: { name: string, gender: Gender }
e = {
  name: 'pd',
  gender: Gender.woman
}
```



### other

```typescript
/**
 * & 且 （脱裤子放屁写法）
 */
let a: { name: string } & { age: number }
a = { name: 'pd', age: 18 } // 必须同时具有
// 正常写法
let a2: { name: string, age: number }
a2 = { name: 'pd', age: 18 }

/**
 * alias 类型的别名
 */
type b = 1 | 2 | 3 | 4
let c: b
c = 1 
// c = 5 // 报错
```



# 编译选项

## 自动编译（热部署）

### 单一文件

编译一个`.ts`文件时，使用**`tsc xxx.ts -w`**（webpack hot）来进行热部署，会在首次编译和之后每次保存的时候自动编译成**JS**文件。

### 多文件

>  当前目录下执行`tsc -init`会创建**`tsconfig.json`**文件（或者手动创建），**可以文件为空**。
>
> > 对当前目录执行`tsc`会对整个目录进行编译一次，执行`tsc -w`会对整个目录进行热部署



## tsconfig.json

```json
{
  /* 用来指定哪些文件需要被编译 */
  "include": [
    "./src/**/*" // ./src/任意目录/任意文件  如果写多个用逗号拼接 "./src/**/*", "../part_1/**/*"
  ],
  /* 编译时排除指定文件 */
  "exclude": [
    // 用法和 include 一样
    // 默认值:["node_modules", "bower_components", "jspm_packages"]  只用默认值时 "exclude" 可以不写
  ],
  /* 继承其他配置文件 */
  // "extends": "./config/base" // 会自动继承bse.json文件中的所有配置信息
}
```

