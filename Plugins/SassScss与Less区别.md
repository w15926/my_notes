# 介绍

Sass (Syntactically Awesome Stylesheets)是一种动态样式语言，Sass语法属于缩排语法，比css比多出好些功能(如变量、嵌套、运算,混入(Mixin)、继承、颜色处理，函数等)，更容易阅读。

Sass的缩排语法，对于写惯css前端的web开发者来说很不直观，也不能将css代码加入到Sass里面，因此Sass语法进行了改良，Sass 3就变成了Scss(Sassy CSS)。SCSS(Sassy CSS)是CSS语法的扩展。这意味着每一个有效的CSS也是一个有效的SCSS语句，与原来的语法兼容，只是用{}取代了原来的缩进。

Less也是一种动态样式语言. 对CSS赋予了动态语言的特性，如变量，继承，运算， 函数.  Less 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可在服务端运行 (借助 Node.js)



# 安装

```shell
npm i sass-loader node-sass --save-dev
```

```shell
npm install less less-loader --save-dev
```

> 注意：loader在vite中可以不用安装进行使用。



# 区别

## 编译环境

Sass是在服务端处理的，以前是Ruby，现在是Dart-Sass或Node-Sass，而Less是需要引入less.js来处理Less代码输出CSS到浏览器，也可以在开发服务器将Less语法编译成css文件，输出CSS文件到生产包目录，有npm less, Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。



## 变量符

Less是@，Sass是$

```scss
// Less-变量定义
@color: #00c;
#footer {
  border: 1px solid @color;
}

// scss-变量定义
$color: #00c;
#footer {
  border: 1px solid $color;
}
```



## mixin

- sass

```css
// 文本溢出省略号隐藏
@mixin textEllipsis($lineCount:1) {
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  word-wrap: break-word;
  -webkit-line-clamp: $lineCount;
  -webkit-box-orient: vertical;
}

// flex自定义 - 改变主轴居中
@mixin felxDirection {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```

- less

```css
// 文本溢出省略号隐藏
.textEllipsis(@lineCount:1) {
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  word-wrap: break-word;
  -webkit-line-clamp: @lineCount;
  -webkit-box-orient: vertical;
}

// flex自定义 - 改变主轴居中
.felxDirection {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```



> Less 调用 mixin 是 .color、.color(1,pink);    Sass 调用则是 @include color、@include color(1,pink);



## 输出设置

Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。

输出样式的风格可以有四种选择，默认为nested
- nested：嵌套缩进的css代码
- expanded：展开的多行css代码
- compact：简洁格式的css代码
- compressed：压缩后的css代码



## 条件语句

Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。

### if-else if-else示例：

```scss
// mixin
@mixin txt($weight) { 
  color: white; 
  @if $weight == bold { 
    font-weight: bold;
  } 
  @else if $weight == light { 
    font-weight: 100;
  } 
  @else { 
    font-weight: normal;
  } 
}

// 调用
.txt1 { 
  @include txt(bold); 
}
```

**编译结果：**

```scss
.txt1 {
  color: white;
  font-weight: bold; 
}
```



### for示例：

```scss
@for $i from 1 to 5 {
  .border-#{$i} {
    border: #{$i}px solid blue;
  }
}
```

**编译结果:**

```scss
.border-1 {
  border: 1px solid blue; }

.border-2 {
  border: 2px solid blue; }

.border-3 {
  border: 3px solid blue; }

.border-4 {
  border: 4px solid blue; }
```



## 引用外部CSS文件

scss@import引用的外部文件如果不想编译时多生成同名的.css文件，命名必须以_开头, 文件名如果以下划线_开头的话，Sass会认为该文件是一个引用文件，不会将其编译为同名css文件.

```scss
// 源代码：
@import "_test1.scss";
@import "_test2.scss";
@import "_test3.scss";
// 编译后：
h1 {
  font-size: 17px;
}
 
h2 {
  font-size: 17px;
}
 
h3 {
  font-size: 17px;
}
```

