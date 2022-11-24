[toc]

# WebGIS单页面分页循环组件框架

> 本框架最终理想结果为在丰富的组件库下开始类似的项目则不需要前端开发人员，只需改些配置即可。



# 修订记录

> 此架构一直做到2.0版本的时候才想起发布到GitHub上，这也是我不断提升的表现。
>
> 有问题可在我GitHub上Issues里进行留言https://github.com/w15926/WebGIS-loopFrame/

|    日期    | 修订版本 |          修改描述          |  作者  |
| :--------: | :------: | :------------------------: | :----: |
| 2022/11/10 |   2.2    |            初稿            | 王乐翔 |
| 2022/11/16 |   2.3    |          架构更新          | 王乐翔 |
| 2022/11/21 |   2.4    |          架构更新          | 王乐翔 |
| 2022/11/21 |  2.4.1   |   处理scss文件引用的问题   | 王乐翔 |
| 2022/11/21 |  2.4.2   | 处理单页面通信多次触发问题 | 王乐翔 |
| 2022/11/23 |  2.4.3   |     处理组件默认值问题     | 王乐翔 |



# 系统配置

> public
>
> > static（静态文件）
> >
> > > config（配置相关）
> > >
> > > > commonConfig.json（公共配置）
> > > >
> > > > interfaces.js（接口环境配置）
> >
> > > bg.png
> >
> > favicon.ico
> >
> > index.html

commonConfig.json

```json
{
  "systemName": "WebGIS单页面分页循环组件框架", // 系统名称
  "systemBg": { // 系统背景（图片覆盖在颜色之上）
    "color": "", // 背景色
    "url": "/static/bg.png" // 背景图片
  },
  "systemCode": "", // 系统编码
  "login": true, // 系统登录
  "homePageIFrame": true, // 首页IFrame动画
  "province": "", // 当前省
  "city": "", // 当前市
  "county": "" // 当前县/区
  // ...
}
```



interfaces.js

```js
/**
 * @description 所有开发生产环境配置
 * @author 王乐翔
 * @createDate 2022/11/04
 * @lastEditDate
 * @lastEditAuthor
 * @returns
 */

let BASE_URL = '' // xxx类
let BASE_URL_g1 = '' // xxx类
let BASE_URL_g2 = '' // xxx类
let BASE_URL_NXDCloud = '' // 公司买的云服务器
let BASE_URL_root = '' // 根路径
let STATIC_URL = '' // 静态目录
let BASE_SOCKETPATH = '' // websocket
let BASE_WECHAT = '' // 微信扫码相关
let isIntranet = null // 是否内网

// xx的（开发环境）
// BASE_URL = 'apis/HistoryCommonWeatherNew'
// BASE_URL_g1 = 'apis/CommonConfigNew-api'
BASE_URL = 'api/HistoryCommonWeather'
BASE_URL_g1 = 'api/CommonConfig-api'

// xx的（生产环境）
// BASE_URL = 'http://192.168.3.222:9000/HistoryCommonWeather'
// BASE_URL_g1 = 'http://192.168.3.222:9000/CommonConfig-api'

window.isIntranet = isIntranet

window.g = {
  AXIOS_TIMEOUT: 10000,
  BASE_API: `${BASE_URL}`,
  STATIC_URL,
  BASE_SOCKETPATH: `${BASE_SOCKETPATH}`,
  BASE_WECHAT: `${BASE_WECHAT}`
}

window.g1 = {
  AXIOS_TIMEOUT: 10000,
  BASE_API: `${BASE_URL_g1}`,
}
```



# 全局methods与配置

在src > utils > methods文件夹下的所有方法通过挂载原型可在任意地方使用（配置档同理）。

方法：src > utils > methods > fn.js

```js
/**
 * @description 生成指定位数 字母数字 验证码 （去除了O、0、i等不好识别的数字和字母）
 * @param { num: Number }
 * @author 王乐翔
 * @createDate 2022/01/13
 * @lastEditDate 2022/03/08
 * @lastEditAuthor 王乐翔
 * @returns 
 */
export const randomCoding = (num = 4) => {
  const arr = ['A', 'a', 'B', 'b', 'C', 'c', 'D', 'd', 'E', 'e', 'F', 'f', 'G', 'g', 'H', 'h', 'J', 'j', 'K', 'k', 'L', 'M', 'm', 'N', 'n', 'P', 'p', 'Q', 'q', 'R', 'r', 'S', 's', 'T', 't', 'U', 'u', 'V', 'v', 'W', 'w', 'X', 'x', 'Y', 'y', 'Z', 'z', '2', '3', '4', '5', '6', '7', '8', '9']
  let verificationCode = ''
  let count = num
  for (let i = 0; i < count; i++) { verificationCode += arr[Math.floor(Math.random() * arr.length)] }
  return verificationCode
}
```



使用

```js
this.$methods // 任意位置调用所有公共方法
this.$config // 任意位置调用/public/static/config下所有json文件
```



# 组件同名目录

自动导入并全局注册components文件夹中的content与public目录下的所有同名目录组件

同名目录组件则为文件夹名与组件名一样，为了进一步严谨，需要在入口文件（同名）加入name属性，name值同为同名。

> public
>
> > MyCom
> >
> > > MyCom.vue（name属性同为MyCom）
> > >
> > > xxx.vue（不需要name）



组件

> 虽然css可以设置scoped，但每个组件需要一个根class，此class名为文件名（同名）

```vue
<template>
	<div class="MyCom" :id="receiveId">
    <!-- ... -->
  </div>
</template>
<script>
export default {
  name: 'MyCom',
  props: {
    receiveId: {
      type: String,
      default: ""
    }
  }
}
</script>
<style lang="scss" scoped>
.MyCom {
}
</style>
```



任意界面使用

```vue
<template>
	<!-- 可在任意界面中直接使用 -->
	<MyCom /> 
</template>
```



# 通过事件总线渲染及交互

因本框架的特殊性，本框架的组件与组件、组件与页面之间的交互统一采用事件总线进行触发，而不是通过父子传参与ref。

开发中每个事件总线必须跟上注释且具有语意化，复杂结构需要采用分类。

如：

```js
this.$bus.$emit('chart_renderTopChart')
this.$bus.$emit('chart_renderRightChart')
this.$bus.$emit('chart_renderBottomChart')
this.$bus.$emit('chart_renderLeftChart')
```



## 通信规范

> xxxIn为触发某个组件内的方法，xxxOut为当前组件向外触发方法。
>
> from不能为空（方便出问题的时候及时排查）； in的时候to必须填写，out的时候to可以不填。
>
> 每个组件的out需在项目**“根目录/busCatalogue.md“**下进行声明。

- 地图

```js
this.$bus.$emit('mapIn',{
    from: '', // 从哪来
    to: null, // 到哪去（为null是指所有组件都可以接受）
    methods: null, // 去的页面要触发的方法
	  triggerIds: '', // 需要触发交互的组件，传组件的receiveId过来，用逗号分隔。
  	// 因JSON.parse(JSON.stringify())存在特殊问题，所以拒绝用它进行深拷贝。
    data :this.$loadsh.cloneDeep(obj) // 传递JS对象并进行深拷贝
})
```

```vue
<template>
  <div class="Test1" ref="Test1">Test1</div>
</template>

<script>
export default {
  name: 'Test1',
  mounted () {
    this.$bus.$on('mapIn', obj => {
      if (obj.to === this.$refs.Map.__vue__.$vnode.componentOptions.tag && this.$refs.Map.__vue__[obj.methods]) {
        this.$refs.Map.__vue__[obj.methods](obj.data)
      }
    })
  }
}
</script>

<style lang="scss" scoped>
.Test1 {
}
</style>
```

```js
this.$bus.$emit('mapOut', ..
```



- 图表

```js
this.$bus.$emit('chartIn', ...
```

```js
this.$bus.$emit('chartOut', ..
```



- 表格

```js
this.$bus.$emit('tableIn', ...
```

```js
this.$bus.$emit('tableOut', ...
```



- 页面

```js
this.$bus.$emit('pageIn', ...
```

```js
this.$bus.$emit('pageOut', ...
```



# 请求/导入JSON文件

使用请求接口方式获取JSON文件

```js
import { reqFile } from '@/utils/request'
```

```js
await reqFile('/static/xxx.json')
```



# 页面介绍

## 目录结构

> views
>
> > page1
> >
> > > page1.vue
> > >
> > 
> >page2
> > 
> >> page2.vue
> > >
> > 
> > page3
> >
> > > page3.vue
> >>
> 
> index.vue

因采用特定的动画效果所以分成了多个文件，每个菜单最大支持三页，每一页都可独立设置地图。

通过获取接口返回数据判断当前页与总页数，并且切换分页的时候采用懒加载模式，极大程度优化了性能。



## 数据说明

每次点击菜单拿到当前菜单数据并调用事件总线进行渲染

```js
res_initPage: {
        ds: [
          // 第一页
          {
            // 左容器组件
            leftComponents: [
              {
                id: 665,
                chName: 'Test3组件',
                province: '江苏省',
                city: '南京市',
                countyOrDistrict: '栖霞区',
                fileCodes: '',
                fileName: 'Test3',
                order: 3,
                paramObject: {},
                categoryCode: 'xxx类的code编码',
                categoryName: 'xxx类',
                time: '2022/11/06 15:35:09',
                margin: '0 0 20px 0',
                triggerIds: '',
                width: '', // 组件宽度（需要修改时使用）
                height: '', // 组件高度（需要修改时使用）
                x: 100,
                y: 100
              }
              // ...
            ],
            // 页面定位组件
            absoluteComponents: [
              {
                id: 666,
                chName: '页面标题组件',
                province: '江苏省',
                city: '南京市',
                countyOrDistrict: '栖霞区',
                fileCodes: '',
                fileName: 'PageTitle',
                order: 1,
                paramObject: { line1: 'b页面-第一页' }, // 实际存储base64字符串
                categoryCode: 'xxx类的code编码',
                categoryName: 'xxx类',
                time: '2022/11/06 15:35:09',
                margin: null,
                triggerIds: '', // 触发交互的组件，传组件的receiveId过来，用逗号分隔。
                width: '',
                height:'',
                x: 40,
                y: 130
              },
              {
                id: 6001,
                chName: '地图',
                province: '江苏省',
                city: '南京市',
                countyOrDistrict: '栖霞区',
                fileCodes: '',
                fileName: 'Map',
                order: 1,
                paramObject: { line1: 'b页面-第一页' },
                categoryCode: 'xxx类的code编码',
                categoryName: 'xxx类',
                time: '2022/11/06 15:35:09',
                margin: null,
                triggerIds: '',
                width: '',
                height:'',
                x: 0,
                y: 0
              }
              // ...
            ],
            // 中间容器组件
            centerComponets: [],
            // 右容器组件
            rightComponents: [],
            // 左容器宽高位置
            leftContainerWidth: '', // 如果不传则自适应
            leftContainerHeight: '800', // 如果不传则自适应，如果存在则自动开启溢出隐藏与鼠标悬浮出滚动条
            leftContainerX: '40',
            leftContainerY: '160',

            // 中间容器宽高位置
            centerContainerWidth: '',
            centerContainerHeight: '',
            centerContainerX: '',
            centerContainerY: '',

            // 右容器宽高位置
            rightContainerWidth: '',
            rightContainerHeight: '',
            rightContainerX: '', // 右容器中X对应right
            rightContainerY: '',
          },
          // 第二页
          {},
          // 第三页
          {}
        ],
        defaultPage: 1, // 默认哪一页
        pageCount: 3, // 总页数
      }
```
