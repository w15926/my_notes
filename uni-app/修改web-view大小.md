# 基本使用

```vue
<template>
  <view class="container-map">
    <layout title="雷电检测预警">
        <view class="web-view">
          <view class="top-box"></view>
          <web-view :src="url"></web-view>
          <view class="bottom-box"></view>
        </view>
    </layout>
  </view>
</template>

<script>
export default {
  data () {
    return {
      wv: null,
      url: "https://www.baidu.com"
    }
  },
  onLoad () {
  },
  onReady () {
    // #ifdef APP-PLUS
    var currentWebview = this.$scope.$getAppWebview();
    const query = uni.createSelectorQuery().in(this);
    var topHeight = 0; var bottomHeight = 0; var boxHeight = 0;
    query.select('.top-box').boundingClientRect(data => {
      topHeight = data.height; // 頭部元素高度
    }).exec();
    query.select('.bottom-box').boundingClientRect(data => {
      bottomHeight = data.height; // 底部元素高度
    }).exec();
    uni.getSystemInfo({
      success (res) {
        boxHeight = res.windowHeight // 屏幕可用高度
      }
    });
    setTimeout(function () {
      var viewHeight = parseInt(boxHeight - topHeight - bottomHeight);
      console.log(viewHeight)
      console.log(topHeight);
      this.wv = currentWebview.children()[0];
      this.wv.setStyle({ top: 90, height: viewHeight - 90 })
    }, 1000); //如果是頁面初始化調用時，需要延時一下
    // #endif
  },
  methods: {
  }
}
</script>

<style lang="scss">
</style>
```



# 根据需求简化

```vue
<template>
  <view class="container-map">
    <layout title="雷电检测预警">
      <view class="web-view">
        <view class="top-box"></view>
        <web-view :src="url"></web-view>
        <view class="bottom-box"></view>
      </view>
    </layout>
  </view>
</template>

<script>
export default {
  data () {
    return {
      wv: null,
      url: "https://www.baidu.com",
    }
  },
  onLoad () {
  },
  onReady () {
    // #ifdef APP-PLUS
    let currentWebview = this.$scope.$getAppWebview();
    let boxHeight = 0
    uni.getSystemInfo({
      success: res => boxHeight = res.windowHeight // 屏幕可用高度
    })
    setTimeout(() => {
      this.wv = currentWebview.children()[0];
      this.wv.setStyle({ top: 90, height: boxHeight - 90 })
    }, 100)
    // #endif
  },
  methods: {
  }
}
</script>

<style lang="scss">

</style>
```

