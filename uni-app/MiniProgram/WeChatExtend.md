# 使用iconfont彩色图标

**Symbol类型，单色的直接复制css**

- 1. 安装插件`npm install mini-program-iconfont-cli --save-dev`
- 2. 生成配置文件`npx iconfont-init`
- 3. 修改根目录生成的`iconfont.json`文件
- - `"trim_icon_prefix": "icon"`的值为`icon-`（常用）
  - `symbol_url`添加在线地址
- 4. 生成小程序组建`npx iconfont-wechat`
- 5. app.json文件中引入全局图标组件，避免每个page都去引入
- - `"usingComponents":{"iconfont": "/iconfont/iconfont"}`
- 6. 在界面中使用`<iconfont name="icon-****"></iconfont>`
- **更新图标：**
- - 修改`symbol_url`再执行`npx iconfont-wechat`



# 使用ES7的async、await

**新版小程序开工具已经更新支持**

> 1. 勾选小程序将JS编译成ES5
> 2. 下载Facebook的regenrator库中的regenrator/packages/regenrator-runtime/runtime.js
> 3. 在小程序目录下新建文件夹`lib/runtime/runtime.js`，将代码拷贝进去
> 4. 在需要用到的界面引入，**不能全局引入**