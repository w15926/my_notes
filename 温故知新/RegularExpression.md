# JS

## 限定符

?

```js
// c 只允许出现 0 次或者 1 次
/abc?d/.test('abcd')
```

*

```js
// c 只允许出现 0 次或者多次
/abc*d/.test('abccd')
```

+

```js
// c 只允许出现 1 次或者多次
/abc+d/.test('abcccd')
```

{ }

```js
// c 只允许出现 3 次
/abc{3}d/.test('abcccd');
// c 只允许出现 1-5 次
/abc{1-5}d/.test('abcccd');
// c 只允许出现 1 次或者以上
/abc{1,}d/.test('abcccd');
```

( )

```js
// 匹配多个字符 匹配 bc 出现 2 次或者以上
/a(bc){2,}d/.test('abcbcd')
```



## 或运算符

|

```js
// a 后面可以跟 bb 或者 cc
/a(bb)|(cc)/.test('acc')
```



## 字符类

[ ] 包含

```js
// 匹配值是否包含 [] 号里的内容
/[abc]/.test('AAAaaa');
/[a-z]/.test('AAAaaa');
/[A-Z]/.test('AAAaaa');
/[a-zA-Z0-9]/.test('AAAaaa');
```

^ 非

```js
// 匹配除 xxx 以外的字符 / 必须包含 xxx 字符
/^[a-z]/.test('aaa001'); // 如果值不包含 a-z 为false
/^[0-5]/.test('5'); // 如果值不包含 0-5 以外为 false
```



## 元字符

> \d 数字符
>
> \w 单词字符 a-zA-Z_
>
> \s 空白字符 （包含换行）



> \D 非数字符
>
> \W 非单词字符
>
> \S 非空白字符



> . 包含任意字符 （除了换行符）
>
> ^a 匹配行首的 a
>
> $a 匹配行尾的 a



## 贪婪与懒惰

贪婪：匹配尽可能多

懒惰：匹配尽可能少

```js
// 匹配所有
/<.+>/.test('<span>this is my pig</span>');
// 匹配所有 < > 及里面的内容
/<.+?>/.test('<span>this is my pig</span>');
```



# Java



# JS 和 Java 中使用区别
