# CSS中使用JS变量

```js
<div class="content" :style="{ '--theme-color': themeColor }">
...
</div>
```

```js
 props: {
    themeColor: {
      type: String,
      default: '#fff'
    }
 }
```

```css
background-color: var(--theme-color);
```

