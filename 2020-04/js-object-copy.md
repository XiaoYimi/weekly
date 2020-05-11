# 对象赋值与深浅拷贝

**知识指引**

基础类型数据的值是存储在栈内存;

引用类型数据的值是存储在堆内存,而指向堆内存的指针存储在栈内存;



## 对象赋值

对象赋值: 是指将一个对象赋值给另一变量;实质上是将这个对象的引用(指针)赋值给变量.



```js
var obj = { a: 1, b: 'hello'}
var nobj = obj;

nobj.a = 12; // obj.a = 12 (指针所指向堆内存的数据发生改变,所有引用该对象的数据也发生变化)
console.log(obj.a) // 12
```





## 对象浅拷贝

对象浅拷贝: 是指新对象仅复制原对象的首层属性,并引用首层属性内部的属性的指针(共享堆内存).

所以新对象修改首层属性,不会影响原对象的改变;但是,对于首层内部属性的修改,会影响到原对象的改变.



```js
var obj = { a: 1, b: { b1: 'hello', b2: 'wold' } }
var nobj = Object.assign({}, obj)

// 确定首层属性 a, b,修改不影响原对象

// 首层内部属性 b.b1, b.b2,修改会影响原对象

nobj.a = 12;
obj.a; // 1

nobj.b.b1 = 'no hello';
obj.b.b1; // 'no hello'
```



### 浅拷贝方法

#### Object.assign()

```js
var obj = { a: 'a', b: { b1: 123, b2: 456 } }
var nobj = Object.assign({}, obj)
```



#### for-in

```js
var obj = { a: 'a', b: { b1: 123, b2: 456 } }

function easyCopy (obj) {
    var res = {}
    for (var key in obj) {
        res[key] = obj[key]
    }
    return res
}

var nobj = easyCopy(obj)
```





## 对象深拷贝

对象深拷贝: 是指完全复制原对象的属性和值,但与原对象不共享同一块堆内存.

所以对新对象的修改,不会影响到原对象值的改变.



```js
var obj = { a: 1, b: { b1: 'hello', b2: 'world' }}
var nobj = JSON.parse(JSON.stringify(obj))

nobj.b.b1 = 7896;
obj.b.b1 = 'hello'
```



### 深拷贝方法

#### JSON.parse(JSON.stringify())

```js
var obj = { a: 1, b: { b1: 'hello', b2: 'world' }}
var nobj = JSON.parse(JSON.stringify(obj))
```



#### for-in 递归

```js
var obj = { a: 'a', b: { b1: 123, b2: 456 } }

function deepCopy (obj) {
    var res = Array.isArray(obj) ? [] : {}
    if (obj && typeof obj === 'object') {
        for (var key in obj) {
            // 避免相互引用所带来的死循环
            var prop = obj[key]
            if (obj === prop) { continue; }
        	if (obj.hasOwnProperty(key)) {
                if (prop && typeof prop === 'object') {
                    // 引用类型
                    res[key] = Array.isArray(prop) ? [] : {}
                    res[key] = arguments.callee(prop)
                } else {
                    // 基础类型
                    res[key] = prop
                }
            }
        }
    }
    return res
}
```



## 小结

- 区分对象赋值与对象深浅拷贝的区别.
- 理解对象浅拷贝对原对象的影响.
- 理解对象深拷贝递归递归的使用.





# About Me

wechat: chenfengbukeyimi

email: 2590856083@qq.com