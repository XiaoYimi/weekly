# 字符串方法大全



## 查询字符首次出现的索引 (indexOf)

```js
var str = 'hello world'
str.indexOf('0') // 4
```



## 合并字符 (concat)

```js
var str1 = 'hello'
var str2 = 'world'
str1.concat(' ', str2) 'hello world'
```



## 返回指定索引的字符 (charAt)

```js
var str = 'hello world'
str.charAt(4) // 'o'
```



## 以开始和结束位置返回字符串 (substring)

```js
var str = 'hello world'
str.substring(3) // 'lo world'
str.substring(2, 5) // 'llo '
```



## 以开始位置和字符长度返回字符串 (substr)

```js
var str = 'hello world'
str.substr(0) // 'hello world'
str.substr(4) // 'o world'
str.substr(0, 2) // 'he'
```



## 字符复制操作 (slice)

```js
var str = 'hello world'
str.slice(3) // 'lo world'
str.slice(2, 5) // 'llo '
```



## 字符分割 (split)

```js
var date = '2020-05-11'
date.split('-') // [2020, 05, 11]

var str = 'abcd'
str.split('') // ['a', 'b', 'c', 'd']
```



## 字符转小写 (toLowerCase)

```js
var str = 'DELLO WORLD'
str.toUpperCase() // 'hello world'
```



## 字符转大写 (toUpperCase)

```js
var str = 'hello world'
str.toLowerCase() // 'HELLO WORLD'
```



## 正则匹配方法-检索一个或多个匹配项 (match)

```js
var str = 'hello world'
str.match(/o/g) // ['o', 'o']
```



## 正则匹配方法-新字符替代匹配项 (replace)

```js
var str = 'hello world'
str.replace(/o/g, '--') // ’hell-- w--rld'
```



## 正则匹配方法-检索匹配项并返回索引 (search)  

```js
var str = 'hello world'
str.search(/o/g) // 4
```

