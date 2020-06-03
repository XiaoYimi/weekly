# JS 常用工具库

## 检测类型 

- Object.prototype.toString.call

```js
const str = 'abc';
const bool = true;
const number = 45;
const symbol = Symbol(78);
const nul = null;
const und = undefined;

const obj = {};
const arr = [];
const reg = /abc/i;
const pro = new Promise((res, rej) => res())
const date = new Date();
const err = new Error();
const set = new Set([45,62,75,85]);

function getDatatype (data) {
	const desc = Object.prototype.toString.call(data).split(' ')[1]
	return desc.slice(0, -1).toLowerCase()
}

function checkDatatype (data, datetype) {
    const type = getDatatype(data)
    return type === datatype
}
```



## 检测移动端浏览器版本

```js
const types = ['trident', 'presto', 'webKit', 'gecko', 'mobile', 'ios', 'android', 'iPhone', 'iPad', 'webApp']

function browserInfo () {
	const ua = navigator.userAgent
	const app = navigator.appVersion
	
    return {
        trident: ua.indexOf('Trident') > -1, //IE内核
        presto: ua.indexOf('Presto') > -1, //opera内核
        webKit: ua.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
        gecko: ua.indexOf('Gecko') > -1 && ua.indexOf('KHTML') === -1, //火狐内核
        mobile: ua.indexOf('AppleWebKit') > -1 && !!ua.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
        ios: !!ua.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
        android: ua.indexOf('Android') > -1 || ua.indexOf('Linux') > -1, //android终端或者uc浏览器
        iPhone: ua.indexOf('iPhone') > -1 || ua.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
        iPad: ua.indexOf('iPad') > -1, //是否iPad
        webApp: ua.indexOf('Safari') === -1 //是否web应该程序，没有头部与底部
    }
	
}
```



## 常用正则验证

```js
const types = ['email', 'phone', 'tel', 'money', 'number', 'password', 'sequence', 'ID', 'IP', 'Date'] 


function checkPattern (data, type) {
	let state = false
	switch (type) {
		case 'money':
			state = /^(0|[1-9][0-9]*)(\.[0-9]{1,2})?$/.test(data)
			break
		case 'email':
			state = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/.test(data)
			break
		case 'phone':
			state = /^1[0-9]{10}$/.test(data)
			break
		case 'tel':
			state = /^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$/.test(data)
			break
		case 'number':
			state = /^\d*(\.\d*)?$/.test(data)
			break
        case 'password':
            state = /^\w{8,}$/.test(data)
            break
        case 'sequence':
            state =  /^\w{4}-\w{4}-\w{4}-\w{4}$/.test(data)
            break
		case 'ID':
			state = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/.test(data)
			break
		case 'IP':
			state = /((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))/.test(data)
			break
		case 'Date':
			state = /^\d{4}-\d{1,2}-\d{1,2}/.test(data)
			break
		default: break
	}
	return state
}
```



## 函数防抖与函数节流

### 函数防抖

#### 概念

事件频繁被触发,但只要在事件停止触发后预定时间中实现业务逻辑.



#### 开发理解注意点

- 函数防抖是指在预定时间后才执行的函数;

- 当在预定时间内多次触发,则重新计算执行时间(移除定时器);

- 当在预定时间内没有再次调用,则执行响应逻辑事件.



#### 开发应解决的问题

1. 解决预定时间后执行响应逻辑事件
2. 解决函数内部 this 指向问题
3. 解决函数 event 指向问题
4. 解决(第三参数)是否立即执行响应逻辑事件
5. 解决响应逻辑事件的返回值问题
6. 解决事件的取消



#### 函数防抖的场景应用

- 搜索框输入搜索
- 表单提交
- 表单验证
- 页面滚动 onscroll
- 页面缩放 onresize



#### 函数防抖的源码实现

```js
function debounce (func, wait, immediate) {
	var timeout, result;
	var debounced = function () {
		// 解决 this 指向
		var ctx = this
		// 解决 event 指向
		var args = arguments
		
		// 立即执行
		if (immediate) {
			var isNow = !timeout
			timeout = setTimeout(function () { timeout = null }, wait)
			if (isNow) { result = func.apply(ctx, args) }
		}
		// 非立即执行
		else {
			timeout = setTimeout(function () {
				func.apply(ctx, args)
			}, wait)
		}
		// 解决响应事件的返回值
		return result
	}
	
	// 定义事件取消
	debounced.cancel = function () {
		clearTimeout(timeout)
		// 防止内存泄漏
		timeout = null
	}
	return debounced
}

/* 应用 */

// 事件内部逻辑
function doSomething () {
    /* do something */
    return 'ok'
}
// 使用函数防抖
const todo = debounce(doSomething, 300, true)
// 触发事件
box.onmousemove = todo
// 取消事件
btn.onclick = function () { todo.cancel() }
```



### 函数节流

#### 概念

事件频繁触发,在频繁触发中间隔 n 秒后实现业务逻辑.



#### 开发理解注意点

- 函数节流是实现周期性操作的函数,即间隔 n  秒操作一次.
- 函数节流它将会立即执行,可通过第三参数 options.leading = false 关闭.
- 函数节流它会在事件结束后再执行一次,可通过第三参数 options.trailing = false 关闭.
- 可通过时间戳形式来定义函数节流,也可以使用定时器来定义函数节流.



#### 开发应解决的问题

1. 解决周期性时间执行响应逻辑事件
2. 解决函数内部 this 指向问题
3. 解决函数 event 指向问题
4. 解决(第三参数)是否执行首次立即响应或结束后不再响应执行.



#### 函数节流的场景应用

- 上拉|触底加载
- 表单输入验证



#### 函数节流的源码实现

```js
// 时间戳实现函数节流 (首次会立即执行)
function throttle (func, wait) {
	var ctx, args
	var pretime = 0
	
	return function () {
		ctx = this
		args = arguments
		var curtime = +new Date()
		
		if (curtime - pretime > wait) {
			pretime = curtime
			func.apply(ctx, args)
		}
	}
}


// 定时器实现函数节流 (结束时会多执行一次)
function throttle (func, wait) {
	var ctx, args, timeid
	
	return function () {
		ctx = this
		args = arguments
		if (!timeid) {
            timeid = setInterval(function () {
                timeid = null
                func.apply(ctx, args)
            }, wait)
        }
	}
}


// 时间戳+定时器双剑合并实现可控制的函数节流
function throttle (func, wait, options) {
    var ctx, args, result, pretime = 0, timeout = null
    if (!options) { options = {} }
    
    return function () {
        var curtime = +new Date()
        ctx = this
        args = arguments
        
        if (!pretime && options.leading === false) { pretime = curtime }
        
        // 时间差值
        var dv = wait - (curtime - pretime)
        
        // 差值大于执行时间,首次立即执行
        if (dv <= 0 || dv > wait) {
            // 清除定时器,并防止内存泄漏
            clearTimeout(timeout)
            timeout = null
            
            pretime = curtime
            result = func.apply(ctx, args)
        }
        // 定时器不存在 且 options.trailing = true
        else if (!timeout && options.trailing !== false) {
            timeout = setTimeout(function () {
                pretime = options.leading === false ? 0 : +new Date()
                timeout = null
                result = func.apply(ctx, args)
            }, dv)
        }
        return result
    }
}
```

