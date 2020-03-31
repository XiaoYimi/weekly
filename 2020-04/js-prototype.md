# 原型对象以及原型链

## 理解原型对象

在 JavaScript 中,当创建一个新函数(FN)时,会自动根据一组特定规则为新函数创建一个属性 prototype,即新函数的对象属性,且这个对象属性会指向它的原型对象(FN.prototype).



在默认情况下,原型对象(FN.prototype)会自动获得一个 constructor (构造函数)属性,且这个属性执行 prototype 属性所在函数的指针(FN).



当使用构造函数(FN)创建一个实例时,该实例的内部包含一个指针(\_\_proto\_\_),且该指针指向构造函数(FN)的原型对象(FN.prototype).



##### demo-1

```js
function FN (name, age) {
	this.name = name;
    this.age = age;
    this.showInfo = function () {
        return [this.name, this.age].join(',') + '.';
    }
}

const fn = new FN('筱依米', 25);

//  FN => 构造函数, FN.prototype => 原型对象, fn => 实例对象

// 构造函数属性(prototype)指向构造函数的原型对象(FN.prototype)
// FN 多加一个括号时为了方便理解
(FN).prototype ===  FN.prototype  // true

// 构造函数的原型对象(FN.prototype)的 constructor 属性指向构造函数所在的指针(FN)
FN.prototype.constructor === FN;  // true

// 实例对象的内部指针(__proto__)指向构造函数的原型对象(FN.prototype)
fn.__proto__ === FN.prototype;   // true


```



## 构造函数,原型对象,实例对象三者之间的关系

- 构造函数通过 new 操作与实例对象保持关联.

- 构造函数通过 prototype 属性与原型对象保持关联.

- 原型对象通过 constructor 属性与构造函数保持关联.

- 实例对象通过 \_\_proto\_\_ 属性与原型对象保持关联. 





## 原型链的基本原理

从实例对象中开始查找相关属性,找到后直接返回该属性值.

如果实例对象中查找不到该属性,则会向与实例对象保持关联的构造函数中(先实例属性,后原型属性)查找.

如果与实例对象保持关联的构造函数中查找不到该属性,则会继续向上找构造函数所继承的对象中(先实例属性,后原型属性)查找.

如果向上一层对象一直找不到,那么将会返回 undefined.





## instanceof 的原理解析

实例对象(fn)的 \_\_proto\_\_ 属性与构造函数无关联,它与构造函数的原型对象(FN.prototype)有所关联.因为实例对象的 \_\_proto\_\_ 属性引用了构造函数的原型对象(FN.prototype).

##### demo-2

```js
fn instanceof FN;  // true
// 实质上是 fn.__proto__ === FN.prototype 的比较
```





## 注意点

- 理解好构造函数的属性(prototype),构造函数的原型对象(FN.prototype),以及实例对象的 \_\_proto\_\_ 属性.
- 只有函数才有 prototype 属性;
- 只有实例对象才会有 \_\_proto\_\_ 属性; function 是 Function 的实例,故 function 也有 \_\_proto\_\_ 属性.
- 原型链属性的查找遵循两原则,查找成功直接返回,否则返回 undefined.
  - 从实例对象自身向上查找.
  - 先实例属性,后原型属性查找.









## About Me

wechat: chenfengbukeyimi

email: 2590856083@qq.com