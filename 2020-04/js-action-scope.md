# 执行环境、作用域与闭包

## 理解执行环境

执行环境规定了变量或函数有权访问的其它数据，并决定各自行为。每个执行环境都有一个变量对象，存储着执行环境内部所定义的变量和函数。



执行环境有全局执行环境和局部(函数)执行环境之分。全局执行环境的变量对象不存在 arguments 对象，而局部(函数)执行环境存在 arguments 对象。



```js
var a = 12;
var b = 'hello';

function func () {
    var c = true;
    return 'ok';
}

// 全局执行环境的变量对象所存储的变量与函数
// 变量: a, b
// 函数: func

// 局部执行环境的变量对象所存储的变量与函数
// 变量: arguments, c
// 函数: null
```



当程序执行到函数时，当前函数的执行环境会被推入栈中；函数执行完毕，函数的执行环境被栈推出，且将控制权返回给之前的执行环境。



## 理解作用域

所谓作用域，是对执行环境内部有权访问所有变量或函数的集合。说到底，就是对执行环境内部的变量或函数的有权访问。



## 理解作用域链

所谓作用域链，是指多层作用域嵌套的说法。而作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。通俗来讲，是指内部执行环境有权访问外部执行环境的变量和函数，而外部执行环境无法访问内部执行环境的变量和函数。



```js
var a = 'a';
function aa () {
    var h = 'h';
    console.log(a);
    return h;
}

function bb () {
    var b = 'b';
    console.log(h);
    return b;
}

aa(); // 'h'
bb(); // 'b'
console.log(h); // h is not defined

// 以上代码涉及到 3 个执行环境，分别是全局执行环境，函数 aa 的局部执行环境以及函数 bb 的局部执行环境。
// 由于作用域链的作用，相对于全局执行环境， 函数 aa 属于内部执行环境，有权访问全局执行环境下的变量 a； 
// 函数 bb 与函数 aa 没有作用域嵌套关系；故无法访问函数 aa 的变量 h；
```



## 延长作用域链

### try-catch 语句

### with 块

```js
function buildUrl (qs) {
    with (location) {
    	var url = href + qs;
	}
    return url;
}
// with 接收 location 对象，其变量对象包含 location 对象的所有属性和方法。
// var url = href + qs; 可以理解为 var url = location.href + qs;
// 使用 with 块延长作用域链；其变量对象呗添加到作用域的前端。引用变量 qs 是来自函数 buildUrl 的形式参数；而引用变量 href，实际上是访问 location.href。 
```



## 没有块级作用域

在其它类 C 语言中，由花括号封闭的代码块都有自己的作用域；在程序执行完毕后就销毁花括号内部所定义的变量。

而在 JavaScript 中，作用域可以理解为执行环境。在程序执行完毕后，所定义的变量和函数仍然保存在变量对象中。



```js
for (var i = 0; i<10; i++) { console.log(i); }
console.log(i); // 10； 变量 i 并没有销毁，而是保存在变量对象中
```



## 作用域链中查找标识符

从作用域链的前端开始，逐级向上查询给定确定的标识符名称。如果找到该标识符，则停止搜索并返回变量值；否则继续沿作用域链向上搜索，一直到全局执行环境。若全局执行环境未找到该标识符，意味着变量尚未声明。





## 理解闭包

所谓闭包，是指有权访问其它函数的内部变量的函数。通俗来讲，就是函数嵌套函数，内部函数持有外部函数的变量的引用。



**闭包所保存的是整个变量对象，而不是某个特殊的值。**



```js
function aa () {
    var a = 'aa 内部变量 a';
    function bb () {
        return a;
    }
    return bb;
}
aa()(); // 'aa 内部变量 a'

// 函数 aa 嵌套 函数 bb；所以函数 aa 为外部函数，函数 bb 为内部函数；由于作用域链的关系；函数 bb 有权访问函数 aa 的变量 a；
// 函数 aa 最终结果返回函数 bb，是一个引用类型，存放在堆内存中；由于函数 bb 依赖于函数 aa 的变量 a；故函数 aa 的变量对象也保存在堆内存中。不会被垃圾回收机制清除。
```



### 闭包的特点

- 函数嵌套函数，内部函数持有外部函数的变量的引用。

- 所引用的变量不会被垃圾回收机制回收。

- 函数内部可以引用外部参数或变量。



### 闭包优点

- 设计私有成员。

- 避免全局变量的污染。

- 有效利用内存中的变量。



### 闭包缺点

- 常驻内存，增加内存使用量。

- 使用不当造成内存泄漏。





## 闭包的应用场景

### 应用场景 -- setTimeout

由于 原生的setTimeout传递的第一个函数不能带参数。 

```js
// before
setTimeout(function (params) {
    console.log(params);
}, 1000);
// 'params si not defind'

// after
function func (params) {
    return function () {
        console.log(params);
    }
}
var fn = func('hello world');
setTimeout(fn, 1000);
// 'hello world'
```



### 应用场景 -- 回调

定义行为，关联到用户事件(click, mousedown)，我们的代码块作为回调处理。

```js
// 定义行为
function bgChange (color) {
    return function () {
        document.body.style.bgColor = color;
    }
}
// 回调处理
var red = bgChange('red');
var yellow = bgChange('yellow');
var blue = bgChange('blue');
// 用户操作
document.getElementById('btn-red').onclick = red;
document.getElementById('btn-yellow').onclick = yellow;
document.getElementById('btn-blue').onclick = blue;
```



### 应用场景 -- 私有成员

使用闭包定义私有成员。

```js
const counter = (function () {
    // 私有成员
   	var count = 0;
    function countChange (val) {
        count += val;
    }
    // 形成闭包
    return {
       inc: function (num) {
           countChange(num);
       },
       dec: function (num) {
           countChange(num);
       },
       val: function () {
           return count;
       }
    }
})();

counter.val(); // 0
counter.inc(10);
counter.val(); // 10
```



### 应用场景 -- 循环节点绑定事件

```js
// 方法一
function showInnerHTML (dom) {
    // 形成闭包
    return function () {
        console.log(dom.innerHTML);
    }
}

function init () {
    const doms = document.getElementById('list');
	for (let i=0; i<doms.children.length; i++) {
        doms.children[i].onclick = showInnerHTML;
    }
}

init();


// 方法二
function showInnerHTML (dom) {
    console.log(dom.innerHTML);
}

function init () {
    const doms = document.getElementById('list');
	for (let i=0; i<doms.children.length; i++) {
        // const 作用域限制在当前块内
        const child = doms.children[i];
        child.onclick = function () {
            showInnerHTML(child);
        };
    }
}
init();
```





## 闭包的问题处理

### 引用(的对象)发生变化

```js
// before
function out () {
    var arr = [];
    for (var i=0; i<10; i++) {
        arr[i] = function () { console.log(i); }
    }
    return arr;
}

const arr = out();
arr[0](); // 10
arr[1](); // 10
arr[2](); // 10

// after 1
function out () {
    var arr = [];
    // let 作用域限制在当前块内
    for (let i=0; i<10; i++) {
        arr[i] = function () { console.log(i); }
    }
    return arr;
}
const arr = out();


// after 2
function out () {
    var arr = [];
    // let 作用域限制在当前块内
    for (var i=0; i<10; i++) {
        arr[i] = function (num) {
            // 形成闭包
            return function () {
                console.log(num);
            }
        }(i);
    }
    return arr;
}
const arr = out();
```



### this 指向

```js
// before
var person = {
    name: 'xiaoyimi',
    getName: function () {
        return function () {
            console.log(this.name); // window.name
        }
    }
};

// after
var person = {
    name: 'xiaoyimi',
    getName: function () {
        const that = this;
        return function () {
            console.log(that.name); // window.name
        }
    }
};
```



### 内存泄漏

内存泄漏是指函数内部变量在程序执行结束时本应销毁，但实际上并没有销毁。

```js
// before 
function showId () {
    var el = document.getElementById('app');
    el.onclick = function () {
        console.log(el.id); // 引用了 el, 程序结束时 el 无法释放
    }
}

// after
function showId () {
    var el = document.getElementById('app');
    var id = el.id;
    el.onclick = function () {
        console.log(id); // 引用了 el, 程序结束时 el 无法释放
    }
    el = null; // 手动释放 el
}
```









# About Me

wechat: chenfengbukeyimi

email: 2590856083@qq.com