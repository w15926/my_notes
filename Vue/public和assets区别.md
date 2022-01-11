# 关于图片

相同点：都是存放静态资源
不同点：
public：图片不会被webpack打包编译，直接复制到最终的打包目录下面
assets：在js中使用的话，路径要经过webpack中的file-loader编译

**注意：**
若把图片放在assets和public中，html页面都可以使用，但是在动态绑定中，assets路径的图片会加载失败（因为webpack使用的是commonJS规范，必须使用require才可以。



> 引用assets中的图片：

```vue
	1、<img src="../assets/logo.png"/> //相对目录
	   <img src="~@/assets/logo.png"/> //绝对目录
	//添加~实际上是为了告诉webpack这里是从根目录开始寻找，而不是相对目录。
	2、使用import引入
		<img :src="ant"/>
		
		import ant from '@/assets/logo.png'
		data(){
		    return {
		        ant:ant
		    }
		}

动态循环assets中的图片：
	<div v-for="(item,index) in 3" :key="index">
	    <img :src="require('@/assets/'+(index+1)+'.png')"/>
	</div>
```



> 引用public下的图片：

```vue
	1、<img src="../../public/static/img/360.png"/>
	2、<img src="/static/img/360.png"/>
	3、import引入
		<img :src="ant"/>
		
		import ant from '../../public/static/img/360.png'
		data(){
		    return {
		        ant:ant
		    }
		}

动态循环public中的图片：
	<div v-for="(item,index) in 3" :key="index">
         <img :src="require('../../public/static/img/'+(index+1)+'.png')"/>
    </div>

```

