# CSS中使用JS变量

> HTML中声明全局CSS变量名

```js
<div class="content" :style="{ '--theme-color': themeColor }">
...
</div>
```

> JS中声明变量值

```js
 props: {
    themeColor: {
      type: String,
      default: '#fff'
    }
 }
```

> CSS中引用全局CSS变量

```css
background-color: var(--theme-color);
```



# 文字溢出省略号

> 单行文本溢出显示省略号 - 必须满足三个条件按

1. 先强制一行内显示文本  `white-space: nowrap; ` （默认normal自动换行）

2. 超出的部分隐藏  `overflow: hidden;`

3. 文字用省略号替代超出的部分  `text-overflow: ellipsis;`



> 多行文本溢出显示省略号，有较大兼容性问题，适合webkit浏览器或移动端（移动端大部分是webkit内核）

```scss
// 方法一
display: -webkit-box;
word-wrap: break-word;
-webkit-line-clamp: 2;
overflow: hidden;
text-overflow: ellipsis;

// 方法二
display: -webkit-box;
-webkit-box-orient: vertical;
word-wrap: break-word;
-webkit-line-clamp: 2;
overflow: hidden;
text-overflow: ellipsis;
```



# 三角形的画法

> 正常三角形

```css
width: 0;
height: 0;
line-height: 0;
font-size: 0;
border: 100px solid transparent;
border-top-color: pink;
border-bottom-color: transparent;
```



> 等腰三角形

```css
width: 0;
height: 0;
line-height: 0;
font-size: 0;
/* 把下面两种颜色改一种透明就可以了 */
/* top像素越大越高，right越大越宽 */
border-top: 90px solid pink;
border-right: 50px solid blueviolet;
```



# 滚动条自定义

> overflow-y: overlay;可以让滚动条不挤压样式，而是覆盖在内容之上。**（Chrome14以后被浏览器淘汰，不再生效。）**

悬浮显示滚动条

```css
    overflow: hidden;
    z-index: 1;
    &::-webkit-scrollbar {
      width: 0px;
    }
    &::-webkit-scrollbar-thumb {
      background: rgba($color: #000000, $alpha: 0.3);
      border-radius: 3px;
    }
    &:hover {
      overflow-x: hidden;
      overflow-y: overlay;
      overscroll-behavior: none;
      &::-webkit-scrollbar {
        width: 6px;
      }
    }
```

默认

```css
      overflow-x: hidden;
      overflow-y: scroll;

      &::-webkit-scrollbar {
        width: 6px;
        height: 6px;

        &-thumb {
          background: rgba($color: #0091FF, $alpha: 0.6);
          border-radius: 3px;
        }
      }
```

# input标签

- 设置placeholder颜色

```scss
input {
  &::placeholder {
    color: #ccc;
  }
}
```



- 去除type为number时的上下箭头

```scss
input {
  // 正常
  &::-webkit-outer-spin-button,
  &::-webkit-inner-spin-button {
    -webkit-appearance: none;Ï
  }
}

// 火狐
input[type="number"] {
  -moz-appearance: textfield;
}
```

