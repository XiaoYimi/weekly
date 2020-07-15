# Cookie 与 Storage



## 理解 Cookie

### Cookie 基本了解

* Cookie 作为 HTTP 的一部分而存在,作用是与服务器进行交互
* 因为 HTTP 协议是无状态的, HTTP 不能对请求或相应进行状态保存
* 由于服务器能读取并设置 Cookies 中的包含信息,并维护用户与服务器件的会话状态
* Cookies 会在每次请求或响应服务器期间携带着用户信息(SessionID)
* Cookies 由服务器生成,有客户端维护和存储
* 根据浏览器不同有不同的个数和长度限制
* 没有现成的 API 接口,需要自行封装
* 使用场景: 登录状态的保存, 用户浏览的数据, 购物车商品记录
* Javascript 无法获取 Cookie 字段的有效时长
* Cookie 有极高的扩展性和可用性(控制 Session 大小, SSL 加密减少 Cookie 窃取, 控制有效时长)
* Cookie 太浪费带宽.



### Cookie 原理

* 第一次请求服务器 => 生成 Cookies(响应头里设置 set-cooike), 并通过响应携带给客户端(保存成 .txt)
* 第 n+1 次请求服务器, 所保存的 Cookies 将随请求携带到服务器,服务器验证用户信息进行数据响应  



### Cookie 属性

* value =>  Cookies 所保存的信息内容(应当加密处理)
* http-only => 无法通过 js (document.cookie) 获取或修改 Cookies, 减少 xss 攻击
* secure  =>  只有协议为 https 才能携带
* same-site  =>  规定不能在跨域请求中携带 Cookies(boolean, null)



### Cookie API 封装

```js
/* 封装 API */

/* 添加 Cookie, expire 为秒 */
const setItem = (name, value, day) => {
  const now = new Date();
  const furter = new Date(now.setDate(now.getDate() + day)).toGMTString();
  document.cookie = `${name}=${value};expires=${furter}`;
};


/* 校验是否存在 Cookie 字段 */
const isExitItem = name => {
  return document.cookie.split(';').some((item) => item.trim().startsWith(`${name}=`));
};

/* 获取 Cookie */
const getItem = name => {
  const cookies = {};
  const groups = document.cookie.split(';').map(item => item.trim());
  groups.forEach(item => {
    const [key, value] = item.split('=');
    cookies[key] = value;
  });
  return cookies[name];
};

const removeItem = name => setItem(name, null, -3);

/* 清空全部 Cookies */
const clearAll = () => {
  const groups = document.cookie.split(';').map(item => item.trim());
  groups.forEach(item => {
    const [key] = item.split('=');
    setItem(key, null, -3);
  });
};

/* 导出模块 */
module.exports = {
  getItem,
  setItem,
  removeItem,
  isExitItem,
  clearAll
};
```





## 理解 Storage

### 本地存储 (localStorage, sessionStorage)

- 为给定的源维持一个独立的存储区域

- 以键值对的形式存储数据,但注意数值型保存后的值为字符串

- localstorage 永久保存在本地, 而 sessionStorage 在浏览器页面会话期间可用(页面加载或恢复)

- 有相应的 API 操作 (getItem, setItem, removeItem, clear).

- 使用场景: 
  - localStorage 统计用户访问当前页面次数; 保存一些固定不变的数据; 存储一些稍微可变的状态数据
  - sessionStorage 进行页面间的传值



