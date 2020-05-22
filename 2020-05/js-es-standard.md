# ES 新特性

## ES3




## ES5

### 严格模式 'use strict';



### JSON 对象方法

- JSON.parse()  ----  反序列化为 JSON 对象.
- JSON.stringify()  ----  序列化为字符串.



### Object 对象方法扩展

```js
Object.getPrototypeOf
Object.getOwnPropertyDescriptor
Object.getOwnPropertyNames    //几乎相同Object.keys但返回对象的所有属性名称（不只是可枚举的）
Object.create    //创建一个新对象，其原型等于proto的值
Object.defineProperty
Object.defineProperties
Object.seal     //密封对象可防止其他代码删除或更改任何对象属性的描述符，以及添加新属性
Object.isSealed        //密封对象可防止其他代码删除或更改任何对象属性的描述符，以及添加新属性
Object.freeze
Object.preventExtensions //关闭可扩展性可能会阻止新属性添加到对象,锁定一个对象，防止属性增加。返回对象
Object.isFrozen     //冻结物体几乎与密封物体相同，但是添加了不可修改的属性
Object.isExtensible  //关闭可扩展性可能会阻止新属性添加到对象，确定对象当前可扩展性的一种方法，返回bool
Object.keys     //返回表示对象的所有可枚举属性名称的字符串数组
Object.hasOwnProperty  //判断是否拥有某个属性
```



### 数组原型对象(Array.prototype)扩展方法(索引,迭代,归并)

```js
Array.prototype.indexOf     //测试一个元素是否存在于一个集合中 Array.prototype.lastIndexOf     //类似indexOf，除了它从数组的末尾开始搜索元素
Array.prototype.every    //集合中的所有项目是否满足指定的条件
Array.prototype.some    //集合中的某些或所有项目是否满足指定的条件
Array.prototype.forEach(function(item,index,Array)) 
Array.prototype.map     //原始数组的转换或映射
Array.prototype.filter  //过滤
Array.prototype.reduce     //将一个数组减少为一个值 reduce(function (previous, current, index, array)，回调函数接受四个参数：上一个值，当前值，索引，以及数组本身
Array.prototype.reduceRight  //将一个数组减少为一个值，工作方式与以下相同reduce，除了它从最后处理数组之外
Array.isArray    //直接将方法写在了构造器上，判断数组
```



返回新函数对象(this 绑定到函数对象实例)

```js
Function.prototype.bind  // 创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
```



#### demo 案例 (this 绑定到第一个参数对象)

```js
// window 浏览器执行
this.name = 'window'

const obj = {
    name: 'object',
    getName () { return this.name }
}

obj.getName()  // 'object'

const newGetName = obj.getName
newGetName() // 'window' // obj.getName 是个方法,但在 window 下使用,this 则为 wondow

newGetName.bind(obj)  // 将 this 绑定到对象 obj; 不管怎么变, newGetname 函数内部的 this 都为 obj
```



#### demo 案例-定义方法预设值

```js
function createList () {
    // 类似 Array.from(arguments).slice()
	return Array.prototype.slice.call(arguments)
}

// 第一个参数后的所有参数都作为预设值
const func_createList = createList.bind(null, 50， 100)

const ori_list = createList(1, 2, 3)
ori_list // [1, 2, 3]

const ins_list = func_createList(45,85,63)
ins_list // [50, 100, 45, 85, 63]
```







## ES6

### let const

- 必须先声明后使用.
- const 一旦定义就无法更改.

```js
const a = 12;
let b = 20;

a = 45; // VM89:1 Uncaught TypeError: Assignment to constant variable.
b = 50; // 50
```



### Map



### Set



### WeakMap



### WeakSet 



### Proxies (Proxy)

#### 使用代理监听对象的操作.





### Promises (Promise)

#### 异步处理对象.



### Symbol

#### 调用 Symbol() 函数返回唯一值.



### 新增对象 API

#### Math



#### String



#### Number



#### Object

- Object.assign()   用于将所有可枚举属性的值从一个或多个源对象复制到目标对象.



#### Array



### 解构赋值

```js
// 对象结构
const obj = { a:1, b:2, c:3, d:4 }
const { a, b, c }= obj

// 数组解构
const arr = [1,2,3,4,5]
const [a, b, , c, d] = arr
console.log(a,b,c,d) // 1,2,4,5
```



### 箭头函数 () => {}

```js
// before
function add (a, b) {
	return a+b
}

// after
const add = (a, b) => a+b
```



### 模板字符串(``, ${})

```js
const { a, b } = { a:1, b:2 }
// before
const desc = 'a:' + a + ', b:' + b + ', total:' + (a+b);

// after
const desc1 = `a:${a}, b:${b}, total:${a+b}`; 
```





### 默认参数值

```js
let count = 0;

// before
function add (num) {
	if (typeof num === 'number') {
		num = num
	} else if (typeof num === 'string') {
		num = parseInt(num)
	} else {
		num = 0
	}
	count = count + num
	return count
}

// after 
const add1 = (num = 0) => {
	if (typeof num === 'string') { num = parseInt(num) }
	count = count + num
	return count
}

add(10) // 10
add() // 10
```



### 展开运算符 (...)

```js
// 与其它数据合并
const color = ['red', ‘orange', 'yellow']
const ncolor = [...color, 'greem'] // ['red', ‘orange', 'yellow', 'green']

// 获取(最后)部分数据
const list = [1, 2, 3, 4, 5, 6, 7]
const [, , lastlist] = list
lastlist // [3, 4, 5, 6, 7]


```





### 剩余参数 (arguments, rest)

```js
// before
function total () {
	this.count = 0
	const total = Array.from(arguments).reduce((prev, next) => { return prev + next })
	console.log(total)
}

// after
const total1 = (...rest) => {
	this.count = 0
	const total = rest.reduce((prev, next) => { return prev + next })
	console.log(total)
}

total(1,2,3,4) // 10
total1(1,2,3,4) // 10
```



### 对象赋值简写

```js
// before
function newObject (name, age) {
	return { name: name, age: age }
}

// after
const newObject1 = (name, age) => {
	return { name, age }
}

newObject('xiaoyimi', 25) // {name: "xiaoyimi", age: 25}
newObject1('xiaoyimi', 25) // {name: "xiaoyimi", age: 25}
```



### Generators (生成器-迭代器函数)

#### 特点

- 普通函数前追加符号 ’*‘;
- 函数内部可使用关键字 yield;
- 每次 yield 后函数会暂停.
- 对异步编程特别友好.



```js
function *createIterator () {
	yield 1;
	yield 2;
	yield 3;
}

const itr = createIterator()
itr.next() // {value: 1, done: false}
itr.next().value // 2
itr.next().value // 3

/* 
每个 yield 都是迭代器函数返回的结果; 
迭代器函数每次只会执行一个 yield 程序段, 且是顺序执行;
只有当迭代器函数内部所有 yield 执行完成后 done 才会自动赋值为 true.
*/
```



#### 迭代器函数 (异步处理多个请求返回)

```js
// 迭代器函数, arr 为所有请求事件的返回值
function *func_itr (arr) {
	for (let i=0; i<arr.length; i++) {
		yield arr[i]
	}
}

var run = (function () {
	function run (func_itr, arr) {
        const list = []
		// 创建任务
		let task = func_itr(arr)
		// 启动任务
		let res = task.next()
		list.push(res.value)
		
		// 创建自动化执行
		function setup () {
			let result = null
			while(!res.done) {
                res = task.next()
                list.push(res.value)
                setup()
			}
		}
		// 启动自动化执行程序
		setup()
		return list.slice(0, list.length -1)
	}
	return run
})()
```





### class、extends、super

```js
class Animal {
	constructor (...rest) {
		this.type = 'animal'
		for (const key in rest) {
			console.log(key)
		}
	}
	
	// 原型(共享)方法
	whatEat () { return 'what is my eat today?' }
}

class Dog extends Animal {
	constructor (...rest) {
		super(rest) // 继承 Animal,并传值给 Animal 实例化
		this.type = 'dog'
	}
	
	eat () { return 'I should be eat bone today' }
}

const dog = new Dog()
dog.type // 'dog'
dog.whatEat() // 'what is my eat today?'
dog.eat() // 'I should be eat bone today'
```





### 模块导入(import)|导出(export) 

```js
// module.js
const count = 12;
var Computed = (() => {
	function Computed () {
		this.count = 0
		if (arguments[0]) {
			for (const key in arguments[0]) {
				this[key] = arguments[0][key]
			}
		}
	}
	
	Computed.prototype.add = function (num) {
		this.count += num
		return this.count
	}
	Computed.prototype.sub = function (num) {
		this.count -= num
		return this.count
	}
	
	return Computed
})()

/* 导出模块 */
export default Computed

/* -------------------------------------------------------------------------- */

// example.js
/* 导入模块 */
import Computed from './xxx/module.js'

const cp = new Computed()
cp.count // 0
```





## ES7

### 指数操作符 (**)



### 新增对象 API

#### Array

- Array.prototype.includes()  检测数组项是否在数组里,返回 Boolean.





### 装饰器(Decorator)

#### 参数:

target  =>  目标类

name  =>  目标类的属性

descriptor  =>  目标类的描述对象







## ES8

### 异步函数 Async|Await

```js
// 函数声明
async function func_name () {}

// 函数表达式
const func_name = async function () {}

// 对象属性方法
const obj = {
	name: 'xiaoyimi',
	async func_name () {}
}

// 箭头函数
const arrow_func = () => {}
```



### 新增对象 API

#### String

- String.padStart(num, char)  对字符串头部进行指定字符(默认空字符)填充指定个数.
- String.padEnd(num, char)  对字符串尾部进行指定字符(默认空字符)填充指定个数.



#### Number



#### Object

- Object.entries()  返回键值对数据结构值.
- Object.values()  返回对象属性值.
- Object.getOwnPropertyDescriptors()  返回目标对象的所有属性的描述符(自定义对象,不继承原型).



### 共享内存与原子 (ShareArrayBuffer, Atomics)



#### 共享内存 (ShareArrayBuffer)

- 表示一个通用的，固定长度的原始二进制数据缓冲区.
- 类似 ArrayBuffer,但不能被分离.

```js
new ShareArrayBuffer(length)
```





#### 原子 (Atomics)

- 针对于 ShareArrayBuffer 实例进行静态操作的对象.
- 不是构造函数,无法 new 实例化.与内置对象 Math 类似使用.

```js
Atomics // {load: ƒ, store: ƒ, add: ƒ, sub: ƒ, and: ƒ, …}
```







## ES9

### async 与 for-of 

```
async function (obj) {
	for (const key of obj) {
		await toDo(obj[key])
	}
}
```



### Promise.finally()

无论 Promise 失败成功,都将会执行此程序块.



### 更加友好的剩余参数 Rest/Spread 

- ES2015 仅支持数组;

```js
const obj = { a:1, b:2, c:3 }
let { a, ...other } = obj
other // {b:2, c:3}


function dealObj ({a, ...other}) {
    console.log(other)
}

dealObj({a: 1, b:2, c:3}) 
//  {b:2, c:3}
```





### 正则表达式

#### 正则表达式命名捕获组 (?\<group_property\>)

```
// before 
const p = /([0-9]{4})-([0-9]{2})-([0-9]{2})/
cosnt m = p.exec('2020-05-22')
const [, group_1, group_2, group_3] = m

group_1 // '2020'
group_2 // '05'
group_3 // '22'

// after
const p = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<date>[0-9]{2})/
const m = p.exec('2020-05-22')
const { year, month, date } = m.groups

year // '2020'
month // '05'
date // '22'
```



#### 正则表达式反向断言

- 这个好绕,后续了解.



#### 正则表达式 dotAll 模式





#### 正则表达式 Unicode 转义



#### 非转义序列的模板字符串





## ES10

### 行分隔符



### 新增对象 API

#### String

String.prototype.matchAll()  返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器.

String.trimStart()  返回从字符串的开头删除空格的字符串, trimLeft() 的别名.

String.tirmEnd()  返回从字符串的结尾删除空格的字符串, trimRight() 的别名.

```js
str = '  srbrt  hrthbbs  rtbrt '
str.trimStart() // 'srbrt  hrthbbs  rtbrt '
str.trimEnd() // '  srbrt  hrthbbs  rtbrt'
```









#### Number



#### Symbol

Symbol.prototype.description



#### Function

Function.prototype.toString()  返回精确字符,包括注释与空格符.



#### Object



#### Array

Array.flat()  对数据对象进行扁平化(1次).

```js
// 参数: num 扁平程度,默认 1. Array.flat(num)

/*
场合:
1. 数组扁平处理
2. 去除空数组项
*/

arr = [1,2,[3,4]]
arr.flat() // [1,2,3,4]

arr = [1,2,[3,4,[5,6]]]
arr.flat() // [1,2,3,4,[5,6]]

// 深层次扁平 Infinity
arr = [1,2,[3,4, [7,8, [9,10, [11,12]]]]]
arr.flat(Infinity) // [1, 2, 3, 4, 7, 8, 9, 10, 11, 12]

// 去除空数组项
arr = [13, , 23, 54, 92, , 23]
arr.flat() // [13, 23, 54, 92, 23]
```



Object.fromEntries() 将键值对转换成 Object 对象.

```js
let entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

let obj = Object.fromEntries(entries)
obj // {foo: "bar", baz: 42}

entries = [['a', 12], ['b', {c: 1}]]
obj = Object.fromEntries(entries)
obj // {a: 12, b: {c: 1}}

// 只转换一层
entries = [['a', 12], ['b', ['c', 45]]]
obj = Object.fromEntries(entries)
obj // {a: 12, b: ['c', 45]}
```





### try-catch 的简化



### 新的基本类型 BigInt



### import()



globalThis:  全局属性 `globalThis` 包含全局的 `this` 值，类似于全局对象(global object).





### 私有实例方法和访问器