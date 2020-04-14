# 创建对象的多种方式以及优缺点

## 创建对象

### 对象字面量

```js
const obj = {
    name: '筱依米',
    age: 25
};
```

特点: 一次性创建对象,复用性低；创建多个类同对象时代码冗余.





### 工厂模式

```js
function factory (name, age, ...rest) {
    const obj = new Object();
    obj.name = name;
    obj.age = age;
    rest.forEach(item => {
        for (const key in item) { obj[key] = item[key]; }
    });
    obj.getInfo = () => {
        const res = [];
        for (const key in obj) {
            typeof obj[key] !== 'function' ? res.push(key + ': ' + obj[key]) : '';
        }
        return res.join(',') + '.';
    }
    return obj;
}

const factory1 = factory('xiaoyimi', 25, {heihgt: 165});
const factory2 = factory('qinmutian', 23, {heihgt: 160});
```

特点: 普通函数,对所有描述对象的属性进行统一处理.





### 构造函数模式

```js
function Person (name, age, ...rest) {
    this.name = name;
    this.age = age;
    rest.forEach(item => {
        for (const key in item) { this[key] = item[key]; }
    });
    this.getInfo = function () {
        const res = [];
        for (const key in this) {
            typeof this[key] !== 'function' ? res.push(key + ': ' + this[key]) : '';
        }
        return res.join(',') + '.';
    }
}

const person1 = new Person('xiaoyimi', 25, {class: 3});
const person2 = new Person('qinmutian', 23, {class: 2});
```

特点: 需 new 操作, this 指向 new 操作创建的实例对象.





### 原型模式

```js
function Pen () {}
Pen.prototype.type = 'pen';
Pen.prototype.write = '使用笔来书写';
Pen.prototype.getInfo = function () {
    return [this.type, this.write].join(',') + '.';
}

const gb = new Pen();
const qb = new Pen();
console.log(gb.__proto__);
console.log(gb.__proto__.constructor);
```

特点: 无法传参初始化,原型对象下的属性和方法共享.



#### 优化一

```js
function Fruit () {}
Fruit.prototype = {
    type: 'fruit',
    eat: function () { return 'eatting fruit...'; }
}

// 重写了 Fruit.prototype, 切断实例对象与原始 Fruit.prototype 的关联(Fruit.prototype.constructor = Object())

const banana = new Fruit();
const orage = new Fruit();
console.log(banana.__proto__);
console.log(banana.__proto__.constructor);
```

特点: 实例与原始原型对象的关联被切断



#### 优化二

```js
function Book () {}
Book.prototype = {
    constructor: Book,
    type: 'book',
    read: function () { return 'reading a book ...'; }
}

// 重写原型对象下的 constructor 属性为构造函数名,使得所创建的实例对象与原始的原型对象保持关联.

const book1 = new Book();
const book2 = new Book();
```

特点: 无法传参初始化,原型对象下的属性和方法共享.





### 构造原型组合模式

```js
function Animal (name, food, ...rest) {
    this.name = name;
    this.food = food;
}

Animal.prototype.type = 'animal';
Animal.prototype.getInfo = function () { return 'The food of ' + this.name + '\' is ' + this.food;  }

const cat = new Animal('折耳猫', 'fish');
const dog = new Animal('柯基犬', 'bone');
```

特点: 保持实例对象的私有属性,也保持了原型下的属性和方法的共享.





### 动态原型模式

```js
function Cat (name, age) {
    this.name = name;
    this.age = age;

    if (typeof this.getInfo !== 'function') {
        // arguments.callee.prototype.getInfo = function () { return [this.name, this.age].join(',') + '.'; }
        this.__proto__.getInfo = function () { return [this.name, this.age].join(',') + '.'; }
    }
}

const dr = new Cat('短耳猫', 3);
const zr = new Cat('折耳猫', 5);
```

特点: if 只会在实例对象调用一次,且该原型下的方法在所有实例上共享.





### 寄生构造函数模式

```js
function Dog (name, age) {
    const obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.getInfo = function () { return [this.name, this.age].join(',') + '.'; }
    return obj;
}

const tyq = new Dog('田园犬', 5);
const kjq = new Dog('柯基犬', 10);
```

特点: 与工厂模式一模一样,唯一区别就是通过 new 操作创建对象.





### 稳妥构造函数模式

```js
function Safe (name, age, classes) {
    const obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.classes = classes;

    obj.getInfo = function () { return [obj.name, obj.age, obj.classes].join(',') + '.'; }
    return obj;
}

const me = Safe('xiaoyimi', 25, 3);
const you = Safe('qinmutian', 23, 2);
```

特点: 内部不使用 this, 创建对象不通过 new 操作.





## 各种创建对象方式的优缺点

#### 对象字面量

- 只能一次性创建对象,不可复用,代码冗余.



#### 工厂模式

- 解决创建一系列相似的对象,但未能解决对象识别问题(都是 Object).实例对象间无关联.



#### 构造函数模式

- 实例可以识别对象的类型,但无法共享属性或方法,实例属性中的方法都是不同的 Function 实例.



#### 原型模式

- 无法传参进行初始化,原型属性值的更改都将在所有实例上有所体现.



#### 构造原型组合模式

- 达到属性私有,方法共享的效果,是目前使用最广泛的方式之一.



#### 动态原型模式

- 内部中的条件判断 if 只会在实例调用一次,且共享在所有实例上.



#### 寄生构造函数

- 解决创建一系列相似的对象,但未能解决对象识别问题(都是 Object).



#### 稳妥构造函数

- 适合在安全环境中使用,但未能解决对象识别问题(都是 Object).







## About Me

wechat: chenfengbukeyimi

email: 2590856083@qq.com

