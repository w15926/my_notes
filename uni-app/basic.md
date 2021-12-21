# 安装使用

> 1. HBuilderX直接创建

> 2. 利用vue-cli创建`vue create -p dcloud/uni-preset-vue 项目名`



# 父子组件

**使用easycom规范（同名目录）可以直接在界面中使用组件，不用再引入**

父

和vue的唯一区别是不用加 :

```vue	
<template>
	<view>
		news
		<card color="tomato" @sonSend="sonSend">
			我是slot默认使用
			<template v-slot:sonName>我是具名插槽</template>
		</card>
		<card color="wheat" size="28px" @sonSend="sonSend"></card>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			sonSend(e) {
				console.log('当前size为：', e);
			}
		}
	}
</script>

<style>

</style>
```



子

```vue
<template>
	<view>
		<view :style="{background: color, fontSize: size}" @click="handleClick">
			card
			<view>color: {{color}}</view>
			<view>size: {{size}}</view>
			<slot></slot>
			<slot name="sonName"></slot>
		</view>
	</view>
</template>

<script>
	export default {
		name: "card",
		props: {
			color: {
				type: String,
				default: 'white'
			},
			size: {
				type: String,
				default: '14px'
			}
		},
		data() {
			return {

			}
		},
		methods: {
			handleClick() {
				this.$emit('sonSend', this.size)
			}
		}
	}
</script>

<style>

</style>
```



# 条件编译

https://uniapp.dcloud.io/platform

**支持**

- .vue
- .js
- .css
- pages.json
- 各预编译语言文件，如：.scss、.less、.stylus、.ts、.pug

标签

```vue
	<!-- 条件编译：仅在App平台显示，更多请参考文档 -->
	<!-- #ifdef APP-PLUS -->
	<button type="default" @click="btn">btn</button>
	<!-- #endif -->

	<!-- #ifndef APP-PLUS -->
	<view>除了App我都显示</view>
	<!-- #endif -->
```

js

````js
  // #ifdef APP-PLUS
  console.log(1);
  // #endif
````

css

```css
	/* #ifdef APP-PLUS */
	.content {
		...
	}
	/* #endif */
```

