# 数组方法大全



## 栈方法 (push, pop)

所谓栈,是一种 ‘后进先出’ 的数据结构.



对数组尾部添加元素  ----  Array.push()

对数组尾部删除元素  ----  Array.pop()





## 队列方法 (shift, unshift)

所谓队列,是一种 '先进先出' 的数据结构.



对数组首部添加元素  ----  Array.unshift()

对数组首部删除元素  ----  Array.shift()



## 数组判断 (isArray)

```js
var arr1 = []
var arr2 = {}
var arr3 = 123

Array.isArray(arr1) // true
Array.isArray(arr2) // false
Array.isArray(arr3) // false
```



## 查询数组项首次出现的索引 (indexOf)

```js
var arr = [1,2,3,4,3,6]

arr.indexOf(3) // 2
```





## 合并数组 (concat)

concat 合并并返回新数组.

```js
var arr1 = [1,2,3]
var arr2 = [4,5,6] 
var arr3 = arr1.concat(arr2) // [1,2,3,4,5,6]
```





## 转换为字符串 (toString, join)

toString 转换为以 ',' 分割的字符串.

```js
var arr = [1,2,3,4,5,6]
arr.toString() // '1,2,3,4,5,6'
```





join 转换为自定义分隔符的字符串.

```js
var arr = [1,2,3,4,5,6]
arr.join('-') // ‘1-2-3-4-5-6’
```





## 数组复制操作 (slice)

slice 截取并返回新数组.

```js
var arr = [1,2,3,4,5,6] 

var arr1 = arr.slice(0,3) // [1,2,3]
```





## 数组增删操作 (splice)

splice 对原数组操作.

```js
var arr = [1,2,3,4,5,6]
// 从某个位置索引删除个数并新增元素
arr.splice(<index>, <number>, <item>)
```





### 对原数组排序 sort

```
var arr = [1,4,6,13,2,7]
arr.sort()
```





## 数组迭代方法 (map,some,every,filter,forEach)

### map 对原数组迭代操作,无返回值



```js
var arr = [1,2,3,4,5,6]
arr.map(item => item * item)
arr // [1,4,9,16,25,36]
```



### some 只要一个为 false,那就返回 false.

```js
var arr = [1,2,3,4,5,6]
arr.some(item => item < 10) // true
arr.some(item => item < 5) // false
```



### every 全部为 true,才返回 true.

```js
var arr = [1,2,3,4,5,6]
arr.every(item => item < 6) // false
array.every(item =>item <= 6) // true
```



### forEach 对数组进行迭代操作,无返回值

 ```js
var arr = [1,2,3,4,5,6]
var arr1 = []
arr.forEach((item, index) => {
    arr1[index] = item
})
arr1 // [1,2,3,4,5,6]
 ```



### filter 迭代过滤,返回满足条件数组项的新数组

```js
var arr = [1,2,3,4,5,6]
var arr1 = arr.filter(item => item % 2 === 0)
arr1 // [0,2,4,6]
```



### reduce 迭代归并,以返回结果作为第一参数传值给下一项,并计算返回

```js
var arr = [1,2,3,4,5,6]

arr.reduce((prev, next) => prev + next) // 21
```

