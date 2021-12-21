# 非小程序版本

1. 官网项目复制 Font class 或者 Symbol 连接
2. 在 App.vue 里使用 `@import url('');`导入链接
3. 页面上使用：如`<text class="iconfont icon-shaixuan"></text>`



**注意：url里引用，在App端如果要显示的话要加`https:`，因为App端的`//`是file协议**

- `@import url('//at.alicdn.com/t/font_2901323_pbixuebjm37.css');`
- `@import url('https://at.alicdn.com/t/font_2901323_pbixuebjm37.css');`



# 小程序版本

1. 在`Unicode`、`Font class`、`Symbol`三个选项中下载第一个至本地
2. 本地不要放在`static`目录下使用（打包不会编译该文件夹，所以不会上传发布），并在`App.vue`引入CSS
3. 复制官网`Unicode`生成的代码（`@font-face`）并Ï覆盖掉本地`iconfont.css`里`@font-face`
4. 重启
5. 页面上使用：如`<text class="iconfont icon-xxx"></text>`
