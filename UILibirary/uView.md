# 基本配置

uni.scss中引入的样式会同时混入到全局样式文件和单独每一个页面的样式中，造成微信程序包太大。

故uni.scss只建议放scss变量名相关样式，其他的样式可以通过main.js或者App.vue引入。



main.js

```js
1.在项目根目录中的main.js中，引入并使用uView的JS库，注意这两行要放在import Vue之后。
// main.js
import uView from "uview-ui";
Vue.use(uView);

2.在引入uView的全局SCSS主题文件
在项目根目录的uni.scss中引入此文件。
/* uni.scss */
@import 'uview-ui/theme.scss';
```



App.vue

```html
// 在App.vue中首行的位置引入，注意给style标签加入lang="scss"属性
<style lang="scss">
    /* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
    @import "uview-ui/index.scss";
</style>
```



pages.json

```json
/*
	* uni-app为了调试性能的原因，修改easycom规则不会实时生效，配置完后，您需要重启HX或者重新编译项目才能正常使用uView的功能。
	* 请确保您的pages.json中只有一个easycom字段，否则请自行合并多个引入规则。
	*/
{
    "easycom": {
       "^u-(.*)": "@/uview-ui/components/u-$1/u-$1.vue"
    },
    
    // 此为本身已有的内容
    "pages": [
       // ......
    ]
}
```