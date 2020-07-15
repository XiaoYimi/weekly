# 构建 Vue 组件库

## 开发前的声明

- 使用 Webpack 构建 关于 Vue 组件的 npm 库模板.
- 使用 npm i <组件库名> 来安装依赖.
- 内部 Vue 单文件组件, 可通过 VUe.use(Plugin) 来加载组件.




## 构建 npm 库

- 在适当的目录下创建 npm 库目录(do-ui), 通过 `npm init` 生成 `package,json` 文件.

```bash
D:> mkdir do-ui
D:> cd do-ui
D:/do-ui> npm init -yes
```



- 在项目根目录下创建 `build `进行 Webpack 配置.

```bash
D:/do-ui> mkdir build
D:/do-ui> touch build/webpack.config.js
```

​	

- 在项目根目录下创建 `src` 目录及主入口文件 `src/index.js` 

```bash
D:/do-ui> mkdir src
D:/do-ui> touch src/index.js
```



- 进入 `src` 目录,创建 `examples`, `plugins`, `assets`,`public`, `router`,`utils` 等目录.

```bash
D:/do-ui> cd src
D:/do-ui/src> mkdir examples plugins assets public router utils
```



​	安装依赖

```bash
npm i webpack webpack-dev-server webpack-merge clean-webpack-plugin babel-loader babel-core style-loader css-loader sass-loader node-sass file-loader url-loader vue-loader vue-template-compiler -D

npm i vue vue-router html-webpack-plugin -S
```



​	创建后的目录结构

```js
- do-ui
	- build
		- webpack.config.js
		- webpack.config.base.js /* 基础配置 */ 
		- webpack.config.devjs /* 开发配置 */
		- webpack.config.pro.js /* 线上配置 */
		- webpack.config.npm.js /* npm 包配置 */
	- src
		- assets /* 静态资源存放地址 */
		- examples /* 实例页面,编写 API 页面 */
		- plugins /* Vue Component 插件(重点) */
		- public /* 存放 index.html, 可存放静态资源 */
		- router /* 路由管理 */
		- utils /* 公共类库方法 */
		- index.js /* 页面主程序入口 */
```



​	webpack.base.config.js 配置内容

```js
const path = require('path')
const HTMLWebpackPlugin = require('html-webpack-plugin')
// 解决编译报错 use VueLoaderPlugin
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'app.js'
  },
  plugins: [
    new VueLoaderPlugin(),
    new HTMLWebpackPlugin({
      template: './src/public/index.html'
    })
  ],
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'], exclude: /node_modules/ },
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'], exclude: /node_modules/ },
      { test: /\.js$/, use: { loader: 'babel-loader' }, exclude: /node_modules/ },
      { test: /\.vue$/, loader: 'vue-loader', options: { loaders: {} }, exclude: /node_modules/ },
      { test: /\.(png|svg|jpeg|jpg|gif)$/, loader: 'file-loader', options: { name: '[name].[ext]?[hash]' }, exclude: /node_modules/ },
      { test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/, loader: 'url-loader', options: { limit: 10000, name: 'fonts/app[hash:7].[ext]' } }
    ]
  },
  resolve: {
    alias: {
      // 解决 Vue.use is not a function, Router is not a constructor 的问题(Vue 版本库 与 Vue 编译器版本必须一致)
      'vue': 'vue/dist/vue.js',
      'vue-router': 'vue-router/dist/vue-router.js',

      // 配置文件路径别名
      '@': path.resolve('src'),
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
}

```



​	webpack.dev.config.js 配置内容

```js
module.exports = {
  devtool: 'cheap-module-eval-source-map'
}
```



​	webpack.pro.config.js 配置内容

```js
const CleanWebpackPlugin = require('clean-webpack-plugin')

module.exports = {
  plugins: [ new CleanWebpackPlugin() ]
}
```



​	webpack.config.js 配置内容

```js
const merge = require('webpack-merge')

const baseConf = require('./webpack.base.config')
const devConf = require('./webpack.dev.config')
const proConf = require('./webpack.pro.config')
// 暂时未用到,后期再补充
// const npmConf = require('./webpack.npm.config')


module.exports = (env, args) => {
  const config = args.mode === 'development' ? devConf : proConf
  return merge(baseConf, config)
}
```



- 配置 `package.json`内`scripts` 脚本指令. 

```json
/* do-ui/package.json */
{
	...
	"scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "webpack-dev-server --mode=development --config ./build/webpack.config.js",
        "build": "webpack --mode=production --config ./build/webpack.config.js",
        "package": "webpack --mode=production --config ./build/webpack.config.js"
      },
    ...
}
```



- 执行脚本语句(测试).

```bash
D:do-ui> npm run start
```




## 编写组件

在目录 src/plugins/components 下创建目录 button 和文件 index.vue

```vue
/* button.vue */
<templete>
  <button @click="hand_btn" :class="btnClass" :disabled="disabled">
  	<slot>Btn</slot>    
  </button>
</templete>

<script>
export default {
  name: 'do-button',
  props: {
    // 按钮状态
    disabled: Boolean,
    // 按钮形状(圆角|直角)
    shape: { type: String, validator: v => (['circle', 'rectangle'].filter(item => item === v)).length },
    // 按钮类型 (success 成功, error 失败, info 信息, warning 警告)
    type: { type: String, validator: v => (['default', 'success', 'error', 'info', 'warning'].filter(item => item === v)).length},
    // 按钮大小
    size: { type: String, validator: v => (['large', 'medium', 'small'].filter(item => item === v)).length },
  },
  data () {
    return {
      originClass: 'do-button'
    };
  },
  methods: {
    hand_btn (ev) { this.$emit('click', ev) }
  },
  computed: {
    btnClass () {
      const { shape, type, size, originClass } = this
      return [
        originClass,
        type ? originClass + '-' + type : originClass + '-' + 'default',
        size ? originClass + '-' + size : '',
        shape ? originClass + '-' + shape : ''
      ]
    }
  }
}
</script>
```



在目录 src/pugins/assets 下创建组件的样式文件.

在目录 src/plugins 下创建文件 style.js, index.js .

```js
/* 所有组件的样式集合 src/plugins/style.js */
@import ('../assets/index.css');
```



```js
/* src/plugins/index.js */
/* 引入所有交互式组件 */
const DoProtoPlugins = require('./prototype/index');

const DoPlugins = {}:

/* Vue 默认安装方法(Vue.use(Plugins)) */
DoPlugins.install = (Vue, option) => {
	const DoComps = require('./compoments/index');
	for (const [k, v] of Object.entries(DoComps)) {
      Vue.component('do' + k, createComponent(v))
    }
};

module.exports = { DoProtoPlugins, DoPlugins };
```



## 使用组件

在 src/main.js 中引入组件样式和组件模板.

```js
/* src/main.js */
import Vue from 'vue';
...

/* 全局使用 start */
/* 加载所有组件及样式 */
import './plugins/assets/scss/index.scss'
const { DoCompPlugins, DoProtoPlugins } = require('./plugins/index');

Vue.use(DoCompPlugins);

// 加载 Vue.prototype.$[name] 方法类型组件
for (const k of Object.values(DoProtoPlugins)) { Vue.use(k) }
/* 全局使用 end */

/* 单独使用 start */
import './plugins/assets/scss/button.scss';
import Button from './plugins/components/button/index.vue';
Vue.use(Button);
/* 单独使用 start */
```

