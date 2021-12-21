https://mp.weixin.qq.com/wxamp/home/guide?lang=zh_CN&token=649917962

AppSecret：09318989729610b9c362218801820644

小程序总代码大小最大支持4MB，所以图片使用在线图片（本地图片可以上传到一些图床网站）

小程序的第三方框架

- 腾讯 wepy 类似 vue
- 美团 mpvue 类似 vue
- 京东 taro 类似 react
- 滴滴 chameleon
- loud uni-app 类似 vue
- 原生框架 MINA

# wxml

## 差异特性

### 在标签属性中使用

```html
<view data-title="{{title}}">在标签属性中使用</view>
```



### 动态绑定

```html
<checkbox checked="{{title}}">动态绑定 - 等于 vue 中 :checked="title"</checkbox>
```



### for

```html
<!-- 标签 for 循环 -->
<view wx:for="{{list}}" wx:for-item="item" wx:for-index="index" wx:key="id">id: {{item.id}}</view>
<!-- 如果只有一层循环，则可以省略 item index -->
<view wx:for="{{list}}" wx:key="id">name: {{item.name}}</view>
<!-- 如果使用普通数组 [1,2,3] 则 *this 代表每一项 -->
<view wx:for="{{arr}}" wx:for-item="item" wx:for-index="index" wx:key="*this">{{item}}</view>
```




### if else hidden(show)

```html
<!-- 标签 if else hidden(vue 中 v-show) -->
<view wx:if="{{ list }}">if</view>
<view wx:else="{{ list }}">else</view>
<view hidden="{{ !list }}">hidden</view>
<view hidden>hidden</view>
```



### 双向绑定

```html		
<!-- 双向绑定 写法相对复杂，等价于 v-model -->
<input type="text" class="input" bindinput="handleInput"/>
<view>data.bind: {{ bind }}</view>
```

```js
  handleInput(e) {
    // 其他框架写法
    // this.data.bind = e.detail.value
    // 小程序写法
    this.setData({
      bind: e.detail.value
    })
  },
```



### 点击事件

```html
<!-- bindtap 点击事件  等价于 @click -->
<!-- 不支持直接传递形参 clickBTN(1) 只能通过自定义参数 data- 方式 -->
<button bindtap="clickBTN" data-param="{{ 1 }}">BUTTON++</button>
```

```js
  clickBTN(e) {
    this.setData({
      bind: this.data.bind += e.currentTarget.dataset.param
    })
  },
```



### text标签

等价 span 标签

```html
<!-- 
  user-select允许用户长按选择
  space显示连续空格
  decode转义字符解码
 -->
<text class="" user-select="true"  space="true" decode="true">
  te       xt&nbsp;text
</text>
```



### view 标签

等价 div 标签



### image标签

https://developers.weixin.qq.com/miniprogram/dev/component/image.html

小程序中此标签默认宽度320px、高度240px

```html
<image class="" src="https://s3.jpg.cm/2021/09/30/Imwp7L.jpg" mode="aspectFit|aspectFill|widthFix" lazy-load="true" binderror="" bindload="" />
```



### navigator标签

https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html

块级元素 类似 a 标签

```html
<!-- 
  target 跳转到哪个小程序 默认 self
  open-type 
    navigate 默认 保留当前页面跳转 不能跳转TabBar
    redirect 关闭当前页跳转 不能跳转TabBar
    switchTab 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
    reLaunch 关闭所有页面，打开到应用内任意的某个页面
 -->
<navigator class="" target="" url="/pages/index/index" hover-class="navigator-hover" open-type="reLaunch">
  点击跳转
</navigator>
```



### rich-text标签

类似 vue v-html

```html
<!-- 
  nodes
    1.接收标签字符串
    2.接收对象数组
 -->
<rich-text class="" nodes="{{html}}" />
```

```js
  data: {
    // 1.标签字符串
    // html: '<div class="sc-bwzfXH Humzi"><div class="sc-bxivhb fUSOkc"></div><div ...... '

    // 2.对象数组 页面显示红色的 hello 文本
    html: [
      {Ï
        // 1.div标签 name属性来指定
        name: 'div',
        // 2.标签上有哪些属性
        attrs: {
          class: 'my_div',
          style: 'color:red;'
        },
        // 3.子节点
        children: [
          {
            name: 'p',
            attrs: {},
            // 放文本
            children: [
              {
                type: 'text',
                text: 'hello'
              }
            ]
          }
        ]
      }
    ]
  },
```



### checkbox标签

<input type="checkbox">

```html
<!-- 行内标签 -->
<checkbox checked="{{title}}">复选框</checkbox>
<!-- 如果跟 boolean 类型就不能在这添加空格，空格默认为 true -->
<checkbox checked="    {{title}}">复选框</checkbox>
<checkbox>复选框</checkbox>

<!-- group块级标签，里面还是行内 -->
<checkbox-group>
  <checkbox checked>复选框</checkbox>
  <checkbox>复选框</checkbox>
</checkbox-group>
```



### radio标签

<input type="radio">

```html
<!-- 单选框 -->
<radio>123</radio>
<radio>123</radio>
<!-- 实现单选 -->
<radio-group bindchange="handleChange">
  <radio color="pink" value="man">man</radio>
  <radio color="pink" value="woman">woman</radio>
</radio-group>
```

```js
  handleChange(e){
    console.log(e.detail.value);
  },
```



### button标签

https://developers.weixin.qq.com/miniprogram/dev/component/button.html

```html
<!-- 
  样式可以自定义
  open-type微信按钮开放样式
    share不能分享到朋友圈
    getPhoneNumber必须企业小程序账号，否则没有权限获取
    launchApp在小程序中打开APP，比较复杂，详情百度
 -->
<button type="primary" loading>BUTTON</button>
<button type="primary" plain loading>BUTTON</button>

<button open-type="contact" type="primary">contact</button>
<button open-type="share" type="primary">share</button>
<button open-type="getPhoneNumber" type="primary" bindgetphonenumber="getPhoneNumber">getPhoneNumber</button>
<button open-type="getUserInfo" type="primary" bindgetuserinfo="getUserInfo">getUserInfo</button>
<button open-type="launchApp" type="primary">launchApp</button>
<button open-type="openSetting" type="primary">openSetting</button>
<button open-type="feedback" type="primary">feedback</button>
```

```js
  getPhoneNumber(e) { 
    console.log(e);
    // 成功后拿 e.detail.encryptedData 发送给服务器端解析
  },
  getUserInfo(e){
    console.log(e.detail.userInfo);
  },
```



## 新特性

### block标签

```html
<!-- block 占位行内标签 输出时隐藏标签及属性，只保留文本 -->
<block wx:for="{{list}}" wx:key="id" class="">name: {{item.name}}</block>
```



### swiper标签

https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html

```html
<swiper indicator-dots indicator-color="rgba(255, 255, 255, .5)" indicator-active-color="rgba(230, 126, 34, 1)" autoplay circular interval="1000">
	<swiper-item class="" item-id="">
		<!-- 写 mode 或者推荐写 wxss 样式 -->
		<image src="//gtms04.alicdn.com/tps/i4/TB1pgxYJXXXXXcAXpXXrVZt0FXX-640-200.jpg" />
	</swiper-item>
	<swiper-item class="" item-id="">
		<image src="//img.alicdn.com/imgextra/i3/17/O1CN01KoNdXZ1BzpRQnKiwo_!!17-0-luban.jpg" />
	</swiper-item>
</swiper>
```



### icon标签

https://developers.weixin.qq.com/miniprogram/dev/component/icon.html

```html
<icon type="info_circle" color="tomato" size="66"/>
```




# wxss


## rpx

```css
.box {
  /*
  1.设计稿 750px
    750px = 750rpx
    1px = 1rpx
  2.把屏幕宽度改成 375px
    375px = 750rpx
    1px = 2rpx
    1rpx = 0.5px
  3.存在一个设计稿 宽度 414 或者 未知 page
    1.设计稿 page 存在一个元素 宽度 100px
    2.拿着以上的需求 去实现 不同宽度的页面适配
    page px = 750rpx
    1px = 750rpx / page
    100px = 750rpx * 100 / page
    假设 page = 375px 
   */

  /* width: 200rpx; */
  width: calc(750rpx * 100 / 375);
  height: 200rpx;
  background-color: pink;
}
```




## 不支持通配符 *

```css
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}
```



## 不支持less

解决方法：vscode -> settings.json

```json
    // Easy Less 插件 编译成 wxss 格式
    "less.compile": {
        "outExt": ".wxss"
    }
```



# 生命周期

## 小程序生命周期

```js
App({
  // 1. 小程序第一次启动
  onLaunch() {
    // 例子：第一次启动的时候获取用户个人信息，方便之后各个节面调用
    console.log('onLaunch');
  },
  // 2. 小程序显示
  onShow() {
    console.log('onShow');
  },
  // 3. 小程序隐藏
  onHide() {
    // 列子：清除定时器
    console.log('onHide');
  },
  // 4. 小程序发生脚本错误，或者 api
  onError(err) {
    // 收集用户信息，发送服务器
    console.log(err);
  },
  // 5. 小程序找不到（入口界面）
  onPageNotFound(e) {
    // 例子 入口界面打不开时手动跳转到指定界面
    // wx.navigateTo({ // 不能跳转 tabBar
    //   url: 'url',
    // })
    console.log('onPageNotFound');
  }
})
```



## 页面生命周期

```js
// pages/demo05/index.js
Page({
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad(options) {
    // 一般发异步请求
  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady() {
    console.log('渲染完成');
  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 监听界面滚动
   */
  onPageScroll(e) {
    console.log(e.scrollTop);
  },

  /**
   * 监听界面尺寸变化
   *   手机屏幕是固定的，一般用来监听横屏竖屏响应
   */
  onResize(e) {
    // 需要在当前界面的 json 文件里配置 "pageOrientation": "auto" 才能监听
    // 或者在 app.json 里配置监听所有界面，配置后模拟器才有旋转功能
    console.log(e);
  },

  /**
   * 监听点击当前 tabBar 时触发
   */
  onTabItemTap(e) {
    console.log(e);
    /*
    index: 0
    pagePath: "pages/index/index"
    text: "首页" 
    */
  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```



## component生命周期

https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/lifetimes.html



# components

## basic

- 1 - 创建组建
- 2 - 在要使用的界面的.json文件中`usingComponents`引用`"Tabs":"../components/Tabs"`
- 3 - 在界面中使用`<Tabs></Tabs>`



## 父传子(component)

- 父

wxml

```html
<Tabs tabs="{{tabs}}"></Tabs>
```

js

```js
tabs: [
      {
        name: 'tabs-1',
        isActive: true
      },
      {
        name: 'tabs-2',
        isActive: false
      },
      {
        name: 'tabs-3',
        isActive: false
      },
      {
        name: 'tabs-4',
        isActive: false
      },
    ]
```



- 子

wxml

```html
<view class="tabs">
  <view class="tabs-title">
    <view class="tabs-item {{item.isActive ? 'tabs-item-active' : ''}}" wx:for="{{tabs}}" wx:key="index" data-index="{{index}}" bindtap="touchTabs">
      {{item.name}}
    </view>
  </view>
</view>
```

wxss

```css
.tabs-title {
  display: flex;
}

.tabs-item {
  flex: 1;
  display: flex;
  justify-content: center;

  padding: 10px;
  background-color: #ccc;
}

.tabs-item-active {
  background-color: #f2f2f2;
  border-bottom: 3px dashed tomato;
}
```

js

```js
// pages/components/Tabs/index.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
    tabs: {
      type: Array,
      value: [{ name: 'empty' }]
    }
  }
})
```



## 子(component)传父

- 子

```js
  /**
   * 组件的方法列表
   */
  methods: {
    touchTabs(e) {
      // 输出index
      const { index } = e.currentTarget.dataset
      console.log('index:', index);

      /*     
        // 激活当前点击，需要 setData 才能推送到 data 里
            // !!!!!! data 里没有会自动去 properties 使用，代码推荐改成this.properties !!!!!!
            // let { tabs } = this.data
            let tabs = JSON.parse(JSON.stringify(this.properties.tabs)) // 严谨写法
            tabs.forEach((v, i) => i === index ? v.isActive = true : v.isActive = false)
      
            // 没有 setProperties 属性，data 中不存在会自动去 properties 中寻找
            this.setData({
              tabs
            }) 
            */

      // 应该通过 子传父 修改
      this.triggerEvent('currentTab', index)

    }
  }
```



- 父

```html
<!-- 类似 vue，子传父跟自定义 key -->
<Tabs tabs="{{tabs}}" bindcurrentTab="changeTab"></Tabs>
```



```js
  changeTab(e) {
    let { detail: index } = e

    let tabs = JSON.parse(JSON.stringify(this.data.tabs)) // 严谨写法
    tabs.forEach((v, i) => i === index ? v.isActive = true : v.isActive = false)

    this.setData({
      tabs
    })
  },
```



## slot

- 子（component）

```html
  <view class="content">
    <slot></slot>
  </view>
```



- 父

```html
<Tabs tabs="{{tabs}}" bindcurrentTab="changeTab">
  <block wx:if="{{tabs[0].isActive}}">page 1</block>
  <block wx:elif="{{tabs[1].isActive}}">page2</block>
  <block wx:elif="{{tabs[2].isActive}}">page 3</block>
  <block wx:else>page 4</block>
</Tabs>
```
