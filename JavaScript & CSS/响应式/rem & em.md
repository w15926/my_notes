# rem

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html {
            /* 根字体一般设置为 10px 好计算 */
            font-size: 10px;
        }
        
        div {
            /* 1rem = 根字体大小  1rem = 10px */
            width: 10rem;
            height: 10rem;
            background-color: wheat;
        }
    </style>
</head>

<body>
    <div>rem</div>
</body>

</html>
```



# em

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .f {
            font-size: 100px;
        }
        
        .f .s {
            /* em 是根据父级来计算的，父级是可以不断继承的，因此开发常用 rem */
            width: 1em;
            height: 1em;
            background-color: wheat;
        }
    </style>
</head>

<body>
    <div class="f">
        <div class="s"></div>
    </div>
</body>

</html>
```



# rem和em的区别

**em是根据父级来计算的，父级是可以不断继承的，因此开发常用 rem**

