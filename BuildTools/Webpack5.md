> 更多见：TypeScript/basic.md



# 安装

**全局安装**

```shell
npm i webpack webpack-cli -g
```

**单独使用的话先初始化NPM**

```shell
npm init -y
```

**项目中安装webpack及webpack-cli**

```shell
npm i webpack webpack-cli -D
```

**指定开发与生产模式**

*需要全局安装*

*项目中会生成打包后的dist文件夹，可执行*

```shell
webpack --mode development
```

```shell
webpack --mode production
```

**指定打包路径（默认index）webpack 要打包的路径 打包后的路径 --mode ...**

```shell
webpack ./src/a.js ./build/build.js --mode ...
```



# 使用

### webpack.config.js

根目录下（src上级）创建webpack.config.js配置文件，可随意修改名称，但是修改后要手动引用

配置文件采用commonJS规范（require）



#### 指定开发与生产模式

```js
module.exports = {
  mode:'production'
}
```



#### 核心配置项

```js
const { resolve } = require('path') // 引用node中path

module.exports = {
  // entry  配置入口文件
  entry: './src/index.js', // 默认就是此路径，可省略或者写其他路径

  // output  配置输出文件
  output: {
    filename: 'build.js',
    path: resolve(__dirname, 'dist') // 必须为绝对路径（绝对路径，输出的文件夹名）
  },

  // loader  webpack默认只能处理 .js 和 .json文件，这里配置其他格式文件
  module:{
    rules:[
      
    ]
  },

  // plugins  功能强大，可直接替代 module
  plugins:[

  ],

  mode: 'production' // 指定开发于生产模式
}
```

*terminal*

```js
webpack // 输入webpack直接运行webpack.config.js里的所有代码
```



#### 多入口与多出口

##### methods-1

```js
  // 多入口（Array）  一起打包成一个chunk，输出一个bundle
  entry: ['./src/index.js', './src/main.js']
```

##### methods-2

```js
  // 多入口（Object）  有几个入口打包成几个chunk，输出为同等多bundle
  entry: {
    one: './src/index.js', // one为自定义打包后的bundle名
    // one: ['./src/index.js', './src/main.js'],
    two: './src/main.js'
  },

  // 需要指定输出方式（多出口）
  output: {
    filename: '[name].js', // 配合多入口（Object）
    path: resolve(__dirname, 'dist')
  },
```



#### 打包HTML

*install*

```shell
npm i html-webpack-plugin -D
```

*use*

```js
const htmlWebpackPlugin = require('html-webpack-plugin')
```

```js
  plugins: [
    // 默认会打包成一个空的index.html，目的是引用JS
    // new htmlWebpackPlugin()

    // 自定义
    new htmlWebpackPlugin({ // 需要打包多少个html就new多少次
      template:'./src/index.html', // 设置要打包文件的路径，会自动引用JS
      filename:'nb666.html', // 打包后的文件名
      minify:{
        collapseWhitespace:true, // 移除空格（生产环境下默认为true）
        removeComments:true // 移除注释（生产环境下默认为true）
      }
    })
  ],
```



#### 打包CSS

*install*

```js
// css-loader：引入外部CSS资源    style-loader：header中插入style标签引用CSS
// 必须js文件中引入css才能生效
npm i css-loader style-loader -D
```

*use*

```js
  module: {
    rules: [
      {
        test: /\.css$/,
        // 在js中引入css，浏览器后F12可以看见style标签
        // 顺序千万不能反（执行顺序从上往下，从右往左）先执行完css再写到header中
        use: ['style-loader', 'css-loader']
      }
    ]
  },
```



#### 提取CSS为单独文件

*install*

```shell
npm i mini-css-extract-plugin -D
```

*use*

```js
const miniCssExtractPlugin = require('mini-css-extract-plugin')
```

```js
  module: {
    rules: [
      // 抽取为单独的CSS文件
      { test: /\.css$/, use: [miniCssExtractPlugin.loader, 'css-loader'] },
      { test: /\.less$/, use: [miniCssExtractPlugin.loader, 'css-loader', 'less-loader'] }
    ]
  },
  plugins: [
    new miniCssExtractPlugin({
      filename: 'deme.css' // 自定义打包后的文件名，默认main.css
    })
  ],
```



#### 处理CSS的兼容性

*install*

```shell
npm i postcss-loader postcss-preset-env -D
```

*use*

```js
  module: {
    rules: [
      // 处理CSS兼容性问题  postcss-loader会自动在根目录下找postcss.config.js配置文件
      { test: /\.css$/, use: [miniCssExtractPlugin.loader, 'css-loader','postcss-loader'] }
    ]
  },
```

*postcss.config.js*

```js
module.exports = {
  plugins: [
    // 需要配合package.json里的browserlist来指定浏览器
    require('postcss-preset-env')()
  ]
}
```



#### 打包Less或者Sass

*install*

```shell
npm i css-loader style-loader -D
```

*use*

```js
  module: {
    rules: [
      // 打包less或者sass时，需要放在最右边，先转译less再执行css，最后写如header标签中
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
    ]
  },
```



#### 压缩CSS

*install*

```shell
npm i optimize-css-assets-webpack-plugin -D
```

*use*

```js
const optimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')
```

```js
  plugins: [
    new optimizeCssAssetsWebpackPlugin()
  ],
```



#### 打包图片资源

*install    url-loader与file-loader为互相依赖关系*

```shell
npm i url-loader file-loader html-loader -D
```

*use*

```js
  module: {
    rules: [
      
      {
        // 对整个文件的图片进行图片
        test: /\.(pnj|jpg|jpeg|gif)$/,
        loader: 'url-loader',
        options: {
          // 打包后dist文件下存放的图片目录
          // base64情况下不生效，不指定limit时默认全部为base64
          outputPath: 'images/',

          // >=8KB 以下的图片才打包成 base64
          limit: 1024 * 8,

          // 打包后图片保持原名非默认hash
          // name: '[name].[ext]'
          name: '[hash:10].[ext]' // 哈希名字中保留十位字母
        }
      },
      
      {
        // 对html文件里引用的图片进行打包  比如<img src="./img/a.jpg" alt="">
        test: /\.html$/,
        loader: 'html-loader', // 处理html里加载的图片  
        options: { esModule: false }// html-loader必选项
      }
      
    ]
  },
```



#### 打包其他资源

**不需要优化和压缩处理，直接输出的资源，称为其他资源**

*install   与打包图片资源用到同样loader*

```shell
npm i file-loader -D
```

*use*

````js
  module: {
    rules: [
      // 打包其他资源字体图标
      {
        // 除这些文件进行打包（不推荐）
        // exclude: /\.(vue|html|js|css|less|scss|json|png|jpg|jpeg|gif)$/,
        // 推荐
        test: /\.(ttf|woff|svg|eot)$/, // 写入字体文件名
        loader: 'file-loader',
        options: {
          // 和打包图片一样
        }
      }
    ]
  },
````



#### 对JS语法配置eslint

*install*

```shell
npm i eslint-loader eslint eslint-config-airbnb-base eslint-plugin-import -D
```

*use*

```js
// package.json
  "eslintConfig": {
    "extends": "airbnb-base"
  }
```

```js
  module: {
    rules: [
      // eslint
      {
        test: /\.js$/,
        // 排除第三方代码
        exclude: /node_modules/,
        loader: 'eslint-loader',
        options: {
          fix: true // 自动修复一些简单错误
        }
      }
    ]
  },
```

