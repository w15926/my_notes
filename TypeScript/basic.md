> 本篇笔记不适合没有后端基础的人群或者懒的人群。



# 环境

- 安装

```shell
npm i -g typescript
```

- 查看版本

```shell
tsc -v
```



# 编译

会转换成x x x.js，和javac一样转换成class后执行。

```shell
tsc xxx.ts
```



## 全局变量提示

TS编译的时候会对变量进行全局声明，即A文件里声明变量`a`后再在B文件里声明变量`a`会报错，并提示变量已经声明。

解决方式之一：

```tsx
export {} // 取消 “无法重新声明块范围变量” 提示
```

> 原理：让tslint以为这是一个ESModule模块，因此不存在变量重复声明的可能性（就变成一个独立的代码块而已）。



## 转换版本

TS默认转换为ES3，可以指定转换成任意ES版本。



# 类型

## 类型声明

- 不声名类型默认隐式为**any**（字面量）

```tsx
let a; // any
a = 10;
a = 'hello';
```



- 声名类型后则必须按照声明的类型进行赋值

```tsx
let a: number;
a = 10;
a = 'hello'; // 报错 
```



- 声明变量时直接赋值

```tsx
let a: number = 10;
```



- 声明变量时不声明类型会存在类型推断

```tsx
let a = 10;
a = '20'; // 报错，此时 a 已隐式声明为 number 类型。
```



- 联合类型（或）

字面量形式

```tsx
let a: '1' | '2' ;
a = '3'; // 报错，类型 '3' 不属于类型 '1' 或 '2'。
```

正常形式

```tsx
let a: string | number;
a = false; // 报错，不能将类型“boolean”分配给类型“string | number”。
```

混合形式（找打形式）

```tsx
let a: '1' | number;
a = '3'; // 报错，类型 '3' 不属于类型 '1' 或 number。
```



- &（且）

很少使用的写法

```tsx
let a: { name: string } & { age: number }
a = { name: 'pd', age: 18 } // 必须同时具有
```

正常写法

```tsx
let a: { name: string, age: number }
a = { name: 'pd', age: 18 }
```



- type

一个公用类型类

```tsx
type myType = number | string;
type myType2 = Array<number> | Array<string>;
let myVar: myType;
let myVar2: myType2;
```





## any 与 unknow

### any

- 关闭当前变量与被使用范围的类型检测

```tsx
let a: any;
console.log(typeof a); // undefined
a = 1
console.log(typeof a); // number
a = '1'
console.log(typeof a); // string
```

影响被赋值的变量

```tsx
let b: number;
b = 1
b = a
console.log(typeof b); // string
```
随便设置一个形参为any类型就能绕过函数返回值的类型

```tsx
function sum(a: number, b: any): number {
  return a + b;
}

console.log(typeof sum(1, '2')); // "12"，string
```



### unknow

- 实际就是安全形势的**any**

```tsx
let a: unknown;
console.log(typeof a); // undefined
a = 1
console.log(typeof a); // number
a = '2'
console.log(typeof a); // string
```

```tsx
let b: number;
b = 1
b = a // 报错，不能将类型“unknown”分配给类型“number”。
```

```tsx
function sum(a: number, b: unknown): number {
  return a + b; // 报错，“b”的类型为“未知”。
}
sum(1, '2')
```

> 因为`unknow`不能直接赋值给其它变量，所以不存在污染。



- 赋值给其它变量的方式

方式一：类型判断

```tsx
if (typeof a === 'number') {
  b = a
}
console.log(b, typeof b); // 1 number
```

> 只有变量`a`类型变成`number`的时候才会赋值

方式二：断言

```tsx
b = a as number
console.log(b, typeof b); // 2 string
```

```tsx
b = b + (a as number)
console.log(b, typeof b); // 12 string
```

方式三：泛型

```tsx
b = <number>a
console.log(b, typeof b); // 2 string
```

```tsx
b = b + <number>a
console.log(b, typeof b); // 12 string
```



> 在方式一与方式二中，使用`as`或`<number>`让当前为`string`类型的变量`a`作为`number`类型强制赋值给变量`b`，但由于变量`a`当前根本不是`number`类型，却又完成了赋值，所以变量`b`的类型也随之转换。
>
> 简单来说就是临时让变量`a`作为`any`类型去进行操作。



## void与never

### void

空值，声明函数类型后仅能返回`undefined`。

- 默认情况

```tsx
function fn(){
}
```

> 函数没有声明类型，且没有返回值，则默认为**void**类型。



- 仅存在返回值情况

```tsx
function fn() {
  return false
}
```

> 此时通过对返回值进行类型推断，未声明类型的函数`fn`变成了`boolean`类型。



- 存在多个返回值却又未声明函数类型的情况下

```tsx
function fn(num: number) {
  if (num) {
    return 123;
  }
  else {
    return '123'
  }
}
```

> 同理，但变成了联合类型，为：function fn(num: number): 123 | "123"。



### never

没有返回值

```tsx
function fn(): never {
  console.log('hello world');
}

fn() // 报错，这种写法的返回值是undefined。空值void也是返回值。
```

一般写法

```tsx
function fn(): never {
  throw new Error('出错了，你明天不用来了！');
}

fn() // 中断程序并返回Error: 出错了，你明天不用来了！
```



## object

- 不应该存在的场景

```tsx
let a: object;
// let a1: {}; // 通过字面量生成对象
a = {};
a = function () { };
a = [];
```

> 因为有着万物皆对象的理念，所以这种形势还不如直接给`any`类型。



- 限制对象中的值

仅能存在一个属性

```tsx
let a: { name: string };
a = { name: 'pd', age: 18 }; // 报错，对象字面量只能指定已知属性。
```

> 只能存在一个`name`属性且值必须为`string`类型。

仅能存在一个必要属性和一个可有可无的属性

```tsx
let a: { name: string, age?: number };
a = { name: 'pd' };
a = { name: 'pd', age: 18 };
```

仅能存在一个必要属性和任何指定类型的属性

```tsx
let a: { name: string, [arg: string]: string };
a = {
  name: 'pd',
  gender: 'woman',
  age: '18'
};
```



## function

- 声明变量为函数类型

```tsx
let fn: (a: number, b: number) => number;
fn = (a, b) => a + b; 
fn(1, 2);
```

> 变量赋值时，形参a、b默认为number类型且不可修改，可不用再写a:number。



## array

- 声明的方式

只能存放string类型的数组

```tsx
let a: string[];
```

```tsx
let a: Array<string>;
```



## tuple

元祖，一种长度及类型固定的数组。

```tsx
let a: [string, number] = ['hello', 10];
```



## enum

全名是enumeration

- 声明枚举类

```tsx
enum Gender {
  Female,
  Male
}
```

> 如果没有赋值，那么第一个key默认值为0（number），然后以此类推。



- 使用

之前写法

```ts
let myBaby: { name: string, age: number, gender: 0 | 1 };
myBaby = {
  name: 'pd',
  age: 18,
  gender: 0
}
```

枚举写法

```tsx
let myBaby: { name: string, age: number, gender: Gender };
myBaby = {
  name: 'pd',
  age: 18,
  gender: Gender.Female
}
```



# 编译选项

## 自动编译（热部署）

- 单一文件

编译一个`.ts`文件时，使用**`tsc xxx.ts -w`**（webpack hot）来进行热部署，会在首次编译和之后每次保存的时候自动编译成**JS**文件。



- 多文件

>  当前目录下执行`tsc -init`会创建**`tsconfig.json`**文件（或者手动创建），**可以文件为空**。
>
> > 对当前目录执行`tsc`会对整个目录进行编译一次，执行`tsc -w`会对整个目录进行热部署



## tsconfig.json

```json
{
  
  "ignoreDeprecations": "5.0",
  
  "include": [],
  "files": [],
  "exclude": [],
  "extends": "",
  "compilerOptions":{
    "target": "",
    "module": "",
    "lib": [],
    "outDir": "",
    "outFile": "",
    "allow": false,
    "checkJS": false,
    "removeComments": false,
    "noEmit": false,
    "noEmitOnError": false,
    "strict": false,
    "alwaysStrict": false,
    "noImplicitAny": false,
    "noImplicitThis": false,
    "strictNullChecks": false
  },
}
```

> - ignoreDeprecations
>
> > 某些属性被弃用导致不允许编译，此项用于忽略弃用去编译。
> >
> > 具体需要填写什么值，鼠标悬浮在报错波浪线上自有提示。
>
> 
>
> - include 
>
> >用来指定哪些文件需要被编译
> >
> >例如：`"./src/**/*"`（./src/任意目录/任意文件）。
>
> 
>
> - files
>
> > 指定某些具体的文件被编译
>
> 
>
> - exclude
>
> > 用来指定哪些文件不需要被编译
> >
> > 默认值：["node_modules", "bower_components", "jspm_packages"]，只使用默认值时，exclude可以不写。
>
> 
>
> - extends
>
> > 会自动继承xxx.json文件中的所有配置信息
>
> 
>
> - compilerOptions
>
> > 编译器配置
> >
> > target
> >
> > > 编译的目标版本，默认值为eS3。
> > >
> > > 如果需要看支持哪些版本，可以书写错误的值编译后看提示即可。
> >
> > module
> >
> > > 各个模块的编译版本，默认为commonjs。
> >
> > lib
> >
> > > 用来指定项目中要使用的库，如`document.xxx`的dom库，一般情况下不需要写lib。
> >
> > outDir
> >
> > > 指定编译后JS文件的路径
> >
> > outFile
> >
> > > 所有代码打包合并到一个JS文件中，但是仅支持"amd" 和 "system" 模块（module里设置）
> >
> > allow
> >
> > > 是否对JS文件进行编译，默认为false（可以用来转换JS版本哦🙂）。
> >
> > checkJS
> >
> > > 是否以TS规范去检查JS文件的代码，默认为false。
> >
> > removeComments
> >
> > > 是否在编译JS时移除注释，默认为false。
> >
> > noEmit
> >
> > > 编译后是否生成JS文件，默认为false（可用来搭配检查JS语法等使用）。
> >
> > noEmitOnError
> >
> > > TS文件有错误时是否中断编译操作，默认为false。
> >
> > strict
> >
> > > 是否一次开启配置中所有的严格检查，默认为false。
> >
> > alwaysStrict
> >
> > > 是否编译成具有严格模式的JS，默认为false。
> >
> > noImplicitAny
> >
> > > 是否允许代码中存在隐式声明的any类型，默认为false。
> >
> > noImplicitThis
> >
> > > 是否允许代码中存在隐式声明或不明确的this，默认为false。
> >
> > strictNullChecks
> >
> > > 是否开启对空值进行严格的检查，默认为false。



## webpack

### 正常模式配置

>  顺便包含了一些非TS相关的配置，在Vue脚手架中不需要这些，因为都帮你配置好了。
>
> 在完整项目的正常模式配置当中，继续增加“跨域配置”与“相对路径配置”即可。



#### 基础配置

```shell
npm i -D webpack webpack-cli typescript ts-loader
```

webpack.config.js

```js
const path = require('path');

module.exports = {
  mode: "production", // development/production
  // 指定入口文件
  entry: './src/index.ts',
  // 指定打包后文件所在目录
  output: {
    // 指定打包后的文件夹名
    path: path.resolve(__dirname, 'dist'),
    // 打包前先清空目录，默认为false。（webpack5以下的版本不支持）
    clean: true,
    // 指定打包后的文件名
    filename: 'bundle.js',
    // 环境设置
    environment:{
       // 打包后是否存在于箭头函数里（兼容IE才需要设置false）
      arrowFunction: false,
    }
  },
  // 设置支持的模块扩展名从而保证import和export可用，默认为 .js 。
  resolve: {
    extensions: ['.ts', '.js']
  },
  // 指定打包时使用的模块
  module: {
    // 指定打包时使用的规则
    rules: [
      {
        // 指定打包时生效的文件
        test: /\.ts$/,
        // 指定对应的loader
        use: 'ts-loader',
        // 指定排除的文件
        exclude: /node_modules/
      }
    ]
  }
}
```

> 运行时在package.json文件里`scripts`节点下加上`"build": "webpack"`，最后运行`npm run build`即可。



#### 指定自动引用编译后的JS文件

```shell
npm i -D html-webpack-plugin
```

webpack.config.js

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: "production", // development/production
  output: {
    // ...
  },
  module: {
    // ...
  },
  // 配置webpack插件
  plugins: [
    new HtmlWebpackPlugin({
      // 声明template时无效
      title: '网页标签页标题', 
      // 自定义使用哪个html模版在里面修改，没有则会创建一个index.html
      template: './src/index.html' 
    })
  ],
}
```



#### 开启开发时的本地服务

```shell
npm i -D webpack-dev-server
```

package.json

```json
  "scripts": {
    "serve": "webpack serve --open",
    // ...
  },
```

> 新增一条serve即可，`--open`为可选，代表运行后是否自动打开，最后直接运行`npm run serve`即可。



#### babel

```shell
npm i -D html-webpack-plugin @babel/core @babel/cli @babel/preset-env babel-loader core-js
```

webpack.config.js

```js
  module: {
    // 指定打包时使用的规则
    rules: [
      {
        // 指定打包时生效的文件
        test: /\.ts$/,
        // 指定对应的loader
        use: [
          'babel-loader', // 使用默认的babel配置
          'ts-loader'
        ],
        // 指定排除的文件
        exclude: /node_modules/
      }
    ]
  },
```

```js
  module: {
    // 指定打包时使用的规则
    rules: [
      {
        // 指定打包时生效的文件
        test: /\.ts$/,
        // 指定对应的loader
        use: [
          'babel-loader', // 使用默认的babel配置
          'ts-loader'
        ],
        // 指定排除的文件
        exclude: /node_modules/
      }
    ]
  },
```



#### 完整代码

```shell
npm i -D webpack webpack-cli typescript ts-loader @babel/core @babel/cli @babel/preset-env babel-loader core-js
```

webpack.config.js

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: "production", // development/production
  // 指定入口文件
  entry: './src/index.ts',
  // 指定打包后文件所在目录
  output: {
    // 指定打包后的文件夹名
    path: path.resolve(__dirname, 'dist'),
    // 打包前先清空目录，默认为false（webpack5以下的版本不支持）。
    clean: true,
    // 指定打包后的文件名
    filename: 'bundle.js',
    // 环境设置
    environment: {
      // 打包后是否存在于箭头函数里（兼容IE才需要设置false）
      arrowFunction: false,
    }
  },
  // 设置支持的模块扩展名从而保证import和export可用，默认为 .js 。
  resolve: {
    extensions: ['.ts', '.js']
  },
  // 指定打包时使用的模块
  module: {
    // 指定打包时使用的规则
    rules: [
      {
        // 指定打包时生效的文件
        test: /\.ts$/,
        // 指定对应的loader
        use: [
          // 配置babel
          {
            loader: 'babel-loader',
            options: {
              // 设置预定义的环境
              presets: [
                [
                  // 指定环境的插件
                  '@babel/preset-env',
                  // 配置信息
                  {
                    targets: {
                      // 兼容的浏览器版本
                      'chrome': '58',
                      'ie': '11',
                      'edge': '17',
                      'safari': '10',
                      'firefox': '60'
                    },
                    // 指定core-js版本
                    "corejs": "3",
                    // 使用 corejs 的方式
                    "useBuiltIns": "usage", // 按需加载 
                  }
                ]
              ]
            }
          },
          'ts-loader'
        ],
        // 指定排除的文件
        exclude: /node_modules/
      }
    ]
  },
  // 配置webpack插件
  plugins: [
    new HtmlWebpackPlugin({
      // title: '网页标签页标题',
      template: './src/index.html'
    })
  ],
}
```

package.json

```json
  "scripts": {
    "serve": "webpack serve --open",
    "build": "webpack"
  },
```



### Vue模式配置



# class

> JS本本就是面向对象，以下用只是我TS再复习一遍。

## 声明、读取、修改

```tsx
// 定义一个Person类
class Person {
  // 定义实例属性
  name: string = '只因'
  // 定义类属性（静态属性）
  static age: number = 18
  // 定义只读实例属性
  readonly sex: string = '男'
  static readonly hobbies: string = '唱、跳、rap'
}

const per = new Person()
console.warn(per);
console.warn(per.name); // 访问实例属性方法
console.warn(Person.age); // 访问静态属性方法

per.name = '坤坤'
Person.age = 20
// per.sex = '女' // 只读无法修改
```



## 方法

```tsx
class Person {
  name: string = '只因'
  static age: number = 18
  readonly sex: string = '男'
  static readonly hobbies: string = 'sing、dance、rap'

  sayHi(): void {
    console.log('Hi, my name is ' + this.name);
  }

  static getHobbies(): void {
    console.log('I like ' + Person.hobbies);

  }
}

const per = new Person()
per.sayHi()
Person.getHobbies()
```



## 构造函数

```tsx
class Pet {
  name: string
  // 构造函数在对象创建时调用
  constructor(name: string) {
    this.name = name
  }
}
const pet = new Pet('小白')
```



## 继承、重写

```tsx
// 父类
class Pet {
  name: string
  age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  sayHi(): string {
    const msg = 'Hi, my name is ' + this.name
    console.log(msg)
    return msg
  }
}

// 子类
class Dog extends Pet {
  // 名字相同会重写，包括构造函数也是。
  sayHi(): string {
    const msg = '你瞅啥？'
    console.log(msg)
    return msg
  }
}

// 子类
class Cat extends Pet {
}

const dog = new Dog('旺财', 2)
const cat = new Cat('大橘', 2)

dog.sayHi() // 你瞅啥？
cat.sayHi() // Hi, my name is 大橘
```



## super

```tsx
class Pet {
  name: string

  constructor(name: string) {
    this.name = name
  }
}

class Dog extends Pet {
  age: number

  // super.sayHi() // 子类调用父类里的某个方法

  // 由于继承后名字一样会重写，所以需要super。
  constructor(name: string, age: number) {
    super(name)
    this.age = age
  }

  sayHi(): string {
    const msg = 'Hi, my name is ' + this.name + ' and I am ' + this.age + ' years old.'
    console.warn(msg)
    return '汪汪汪'
  }
}

const dog = new Dog('旺财', 2)
dog.sayHi() // Hi, my name is 旺财 and I am 2 years old.
```

