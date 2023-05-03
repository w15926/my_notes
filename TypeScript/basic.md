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
