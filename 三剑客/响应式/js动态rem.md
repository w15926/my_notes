```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        let screen = () => {
            // 获取屏幕宽度
            let screenWidth = document.documentElement.clientWidth;
            // 动态计算 10 = font-size / 320 = 当前设计稿宽度 / 40 = 最大font-size 
            let changeScreenWidth = (10 * (screenWidth / 320) > 40 ? 40 + 'px' : (10 * (screenWidth / 320) + 'px'))
            document.documentElement.style.fontSize = changeScreenWidth
        }
        window.addEventListener('load', screen)
        window.addEventListener('resize', screen)
    </script>
    <style>
        html {
            /* 设置js后此样式失效 */
            font-size: 10px;
        }
        
        div {
            font-size: 1rem;
        }
    </style>
</head>

<body>
    <div>hello world</div>
</body>

</html>
```

