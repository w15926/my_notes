# Basic information

### Install

```shell
npm i -g create-vite-app
```

### Create project

> npm create 只是 npm init 的别名而已。

```shell
# vite创建各种类型项目
npm create vite@latest projectName
# npm 6.x
npm create vite@latest my-vue-app --template vue
# npm 7+
npm create vite@latest my-vue-app -- --template vue
# 默认创建使用vite的Vue3项目，格式与Vue create创建项目类似
npm create vue projectName

#Vue官方创建Vite项目
npm init vue@latest
```



# Difference from Webpack



# 打包成ES5

npm

```shell
npm install vite @vitejs/plugin-legacy -D
```

vite.config

```shell
import legacy from '@vitejs/plugin-legacy'
```

```js
  plugins: [
    legacy({
      targets: ['defaults', 'not IE 11']
    })
  ]
```

package.json

```json
  "browserslist": [
    "defaults",
    "not IE 11"
  ]
```

**注意：build失败可以降级vite版本，`"vite": "5.4.11"`，然后重新执行`npm i`。**