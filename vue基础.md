## 模块化介绍

### 模块化的好处

> 回顾nodejs中模块化的使用

node.js 遵循了 CommonJS 的模块化规范。其中：

+ 导入其它模块使用 `require()` 方法
+ 模块对外共享成员使用`module.exports` 对象

模块化的好处：

+ 模块化可以避免命名冲突的问题
+ 大家都遵守同样的模块化规范写代码，**降低了沟通的成本，极大方便了各个模块之间的相互调用**
+ 只需关心当前模块本身的功能开发，需要其他模块的支持时，在模块内调用目标模块即可

### 为什么要学习ES6 模块化规范

ES6 模块化规范是浏览器端与服务器端通用的模块化开发规范。它的出现极大的降低了前端开发者的模块化学
习成本，开发者不需再额外学习 AMD、CMD 或 CommonJS 等模块化规范。

ES6 模块化规范中定义：

+ 每个 js 文件都是一个独立的模块
+ 导入其它模块成员使用 `import`  关键字
+ 向外共享模块成员使用 `export` 关键字 

## ES6模块语法

ES6 的模块化主要包含如下 3 种用法：

+ 默认导出与默认导入
+ 按需导出与按需导入
+ 直接导入并执行模块中的代码

### 默认导出与默认导入

默认导出的语法： `export default 默认导出的成员`

默认导入的语法： `import 接收名称 from '模块路径'`

+ 导出

```jsx
const a = 10
const b = 20

const fn  = () => {
  console.log('这是一个函数')
}

// 默认导出
// export default a  // 导出一个值
export default {
  a,
  b,
  fn
}
```

+ 导入

```jsx
// 默认导入时的接收名称可以任意名称，只要是合法的成员名称即可
import result from './xxx.js'
console.log(result)
```

**注意点:** 

- 每个模块中，只允许使用唯一的一次 `export default `
- 默认导入时的接收名称可以任意名称，只要是合法的成员名称即可

### 按需导入与按需导出

按需导出的语法： `export const s1 = 10`

按需导入的语法： `import { 按需导入的名称 } from '模块标识符'`

```jsx
export const a = 10
export const b = 20
export const fn = () => {
  console.log('内容')
}
```

按需导入的语法

```jsx
import { a, b as c, fn } from './xxx.js'
```

注意事项：

+ 每个模块中可以有多次按需导出
+ 按需导入的成员名称必须和按需导出的名称保持一致
+ 按需导入时，可以使用as 关键字进行重命名
+ 按需导入可以和默认导入一起使用

### 直接导入模块(无导出)

如果只想单纯地执行某个模块中的代码，并不需要得到模块中向外共享的成员。

此时，可以直接导入并执行模块代码，示例代码如下：

`import '模块的路径'`

```jsx
//xxx.js
for (let i = 0; i < 10; i++) {
  console.log(i)
}


// 导入该模块
import './xxx.js'
```

# EventLoop

##  JavaScript 是单线程的语言

> JavaScript 是一门单线程执行的编程语言。也就是说，同一时间只能做一件事情。

单线程执行任务队列的问题：
如果前一个任务非常耗时，则后续的任务就不得不一直等待，从而导致**程序假死**的问题。



## 同步任务和异步任务

为了防止某个耗时任务导致程序假死的问题，JavaScript 把待执行的任务分为了两类：

+ 同步任务（synchronous）
  + 又叫做非耗时任务，指的是在主线程上排队执行的那些任务
  + 只有前一个任务执行完毕，才能执行后一个任务
+ 异步任务（asynchronous）
  + 又叫做耗时任务，异步任务由JavaScript 委托给宿主环境(浏览器 nodejs)进行执行
  + 当异步任务执行完成后，会通知 JavaScript 主线程执行异步任务的回调函数

## EventLoop执行过程

代码示例：

```javascript
console.log(1)

setTimeout(() => {
  console.log(2)
})


console.log(3)
```



+ 同步任务由 JavaScript 主线程次序执行
+ 异步任务委托给宿主环境执行
+ 已完成的异步任务对应的回调函数，会被加入到任务队列中等待执行
+ JavaScript 主线程的执行栈被清空后，会读取任务队列中的回调函数，次序执行
+ JavaScript 主线程不断重复上面的第 4 步

JavaScript 主线程从“任务队列”中读取异步任务的回调函数，放到执行栈中依次执行。这个过程是循环不断的，所以整个的

这种运行机制又称为 EventLoop（事件循环）。

# Promise

## 异步与回调函数

异步函数:  setTimeout  setInterval  ajax   fs.readFile  addEventListener

回调函数:

1. **把一个函数当成参数传递, 将来特定的时机调用, 这个函数就叫回调函数**     
2. 一般什么时候会用到回调函数———— 异步的时候 ：  定时器， ajax(success, error)

```jsx
function fn(a) {
  // console.log(a)
  setTimeout(() => {
    a()
  }, 1000)
}
fn(function () {
  console.log('哈哈')
})
```

回调函数的问题:

1. 回调函数的阅读性不好, 回调不会立马执行
2. 回调函数如果有大量的嵌套, 可维护性差  

## 回调地狱

多层回调函数的相互嵌套，就形成了回调地狱。示例代码如下

```jsx
import fs from 'fs'

fs.readFile('./files/1.txt', 'utf8', (err, data) => {
  if (err) return console.log(err)
  console.log(data)
  fs.readFile('./files/2.txt', 'utf8', (err, data) => {
    if (err) return console.log(err)
    console.log(data)

    fs.readFile('./files/3.txt', 'utf8', (err, data) => {
      if (err) return console.log(err)
      console.log(data)
    })
  })
})

```

回调地狱的缺点：

+ 代码耦合性太强，牵一发而动全身，难以维护
+ 大量冗余的代码相互嵌套，代码的可读性变差

## promise基本概念

> 为了解决回调地狱的问题，ES6（ECMAScript 2015）中新增了 Promise 的概念。
>
> promise：承诺  许诺

**promise有三种状态**

- pending:  等待 (进行中)

- fulfilled: 成功 (已完成), 调用了 resolve, promise的状态就会被标记成成功

- rejected: 失败 (拒绝), 调用了 reject, promise的状态就会被标记成失败

**小tips: 一旦promise的状态发生变化, 状态就会被凝固**

## promise基本使用

+ Promise 是一个构造函数
  + 我们可以创建 Promise 的实例 const p = new Promise() 
  + new 出来的 Promise 实例对象，代表一个异步操作
+ Promise.prototype 上包含一个 .then() 方法
  + 每一次 new Promise() 构造函数得到的实例对象，
  + 都可以通过原型链的方式访问到 .then() 方法，例如 p.then()
  + 通过.then方法来指定成功的回调函数
+ Promise.prototype 上包含一个 .catch() 方法
  + 都可以通过原型链的方式访问到 .then() 方法，例如 p.then()
  + 通过.catch方法来指定失败的回调函数

```js
const p = new Promise(function (resolve, reject) {
  // promise 内部会封装一个异步的操作
  // resolve: 成功的时候, 需要调用
  // reject: 失败的时候, 需要调用
  fs.readFile('a.txt', 'utf8', (err, data) => {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

p.then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

## Promise的说明

> 正常开发中，我们封装promise的场景会比较少，更多的都是使用一些第三方的库，这些库已经基于promise语法对异步进行了封装，所以我们的关注点应该是在promise对象的使用上，而不是封装上。

由于 node.js 官方提供的 fs 模块仅支持以回调函数的方式读取文件，不支持Promise 的调用方式。因此，需
要先运行如下的命令，安装 then-fs 这个第三方包，从而支持我们基于 Promise 的方式读取文件的内容：

```jsx
npm i then-fs
```

读文件基本使用

```jsx
import fs from 'then-fs'

fs.readFile('./files/1.txt', 'utf8')
  .then((data) => {
    console.log(data)
  })
  .catch((err) => {
    console.log(err)
  })


```



## Promise的链式调用

`Promise 支持链式调用`，从而来解决回调地狱的问题。

如果上一个 .then() 方法中返回了一个新的Promise 实例对象，则可以通过下一个 .then() 继续进行处理

```jsx
import fs from 'then-fs'

// promise支持链式调用，而不用嵌套调用
fs.readFile('./files/1.txt', 'utf8').then(r1 => {
  console.log(r1)
  return fs.readFile('./files/2.txt', 'utf8')
}).then(r2 => {
  console.log(r2)
  return fs.readFile('./files/3.txt', 'utf8')
}).then(r3 => {
  console.log(r3)
}).catch(err => {
  console.log(err)
})

```

## promise静态方法

+ `Promise.all([ promise1, promise2, ... ]).then( ... )`

  Promise.all() 方法会发起并行的 Promise 异步操作，等所有的异步操作全部结束后

  才会执行下一步的 .then操作（**等待机制**）。

  ```javascript
  import fs from 'then-fs'
  Promise.all([
    fs.readFile('./1.txt', 'utf8'), 
    fs.readFile('./2.txt', 'utf8'), 
    fs.readFile('./3.txt', 'utf8')]
  ).then(data => {
    console.log('所有文件都完成')
    console.log(data)
  }).catch(err => {
    console.log('读取失败')
    console.log(err)
  })
  ```

  

+ `Promise.race([ promise1, promise2, ... ]).then( .... )`

  Promise.race() 方法会发起并行的 Promise 异步操作，只要任何一个异步操作完成，

  就立即执行下一步的 .then 操作（**赛跑机制**）

  ```javascript
  
  const promise1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 500, 'one');
  });
  
  const promise2 = new Promise((resolve, reject) => {
    setTimeout(reject, 100, 'two');
  });
  
  Promise.race([promise1, promise2]).then(data => {
    console.log('成功')
    console.log(data)
  }).catch(err => {
    console.log('失败')
    console.log(err)
  })
  ```

  

+ `Promise.resolve()`返回一个只会成功的promise对象

  ```javascript
  Promise.resolve()  // 直接返回一个只会成功的promise对象
  Promise.reslove(100)
  
  //等同于
  new Promise(function (resolve, reject) {
    resolve(100)
  })
  ```

  

+ `Promise.reject()`返回一个只会失败的Promise对象

  ```javascript
  Promise.reject()  // 直接返回一个只会失败的promise对象
  Promise.reject(100)
  
  //等同于
  new Promise(function (resolve, reject) {
    reject(100)
  })
  ```

+ `Promise.resolve()`和`Promise.reject()` 面试题举例

  ```javascript
  console.log(1)
  setTimeout(function () {
    console.log(2)
  })
  Promise.resolve(5).then(data => {
    console.log(data)
  })
  console.log(3)
  ```

  

# async与await

## 基本概念 

> async await号称异步的终极解决方案，async await之后再无回调

async/await 是 ES8（ECMAScript 2017）引入的新语法，用来简化 Promise 异步操作。在 async/await 出
现之前，开发者只能通过链式.then() 的方式处理 Promise 异步操作。

+ .then方式的优点：
   + 解决了回调地狱的问题
 + .then方式的缺点
    + 代码冗余、阅读性差、不易理解

## 基本使用 

async  和 await 是一对关键字

1. async用于修饰一个函数, 表示一个函数是异步的

   如果async函数内没有await, 那么async没有意义的, 全是同步的内容

   只有遇到了await开始往下, 才是异步的开始

```js
console.log(1)
async function fn () {
  console.log(4)
  const res = await 2
  console.log(res)
}
fn()
console.log(3)
```

2. await 要用在 async 函数中

3. await 后面一般会跟一个promise对象,  await会阻塞async函数的执行,

   直到等到 promise成功的结果(resolve的结果)

````jsx
// 1. async和await是一对关键字，必须成对出现
// 2. async用于修饰一个函数,代表该函数是一个异步函数
//    注意：只有await往后的才是异步代码
// 3. await用于等待一个值，通常是一个promise
// 4. await后面是一个普通值，那会直接返回该值本身，如果await后面是一个promise,会返回promise成功的结果

import fs from 'then-fs'
// fs.readFile('./files/1.txt').then(data = >)
async function fn() {
  // console.log(3)
  const data = await fs.readFile('./files/1.txt', 'utf8')
  console.log(data)
  const r2 = await fs.readFile('./files/2.txt', 'utf8')
  console.log(r2)
  const r3 = await fs.readFile('./files/3.txt', 'utf8')
  console.log(r3)
}
fn()
console.log(2)
````

## 异常处理

await 只会等待 promise 成功的结果, 如果失败了会报错, 需要 try catch

```jsx
// 1. async和await是一对关键字，必须成对出现
// 2. async用于修饰一个函数,代表该函数是一个异步函数
//    注意：只有await往后的才是异步代码
// 3. await用于等待一个值，通常是一个promise
// 4. await后面是一个普通值，那会直接返回该值本身，如果await后面是一个promise,会返回promise成功的结果

import fs from 'then-fs'
// fs.readFile('./files/1.txt').then(data = >)
async function fn() {
  // console.log(3)
  const data = await fs.readFile('./files/1.txt', 'utf8')
  console.log(data)
  try {
    const r2 = await fs.readFile('./files/22.txt', 'utf8')
    console.log(r2)
  } catch (err) {
    console.log('读取文件失败了', err)
  }
  const r3 = await fs.readFile('./files/3.txt', 'utf8')
  console.log(r3)
}
fn()
console.log(2)
```

# 宏任务和微任务

## 基本概念

JavaScript 把任务队列分为**宏任务队列**和**微任务队列**

+ 宏任务（macrotask）
  + 整个script标签
  + 异步 Ajax 请求、
  + setTimeout、setInterval、
  + 文件操作
  + 其它宏任务
+ 微任务（microtask）
  + Promise
  + process.nextTick
  + 其它微任务

每一个宏任务执行完之后，都会检查是否存在待执行的微任务

如果有，则执行完所有微任务之后，再继续执行下一个宏任务。

全是宏任务：

```jsx
// 宏任务1
console.log(1)

// 只要看到了加了时间, 那么一定是等时间满足了, 才会加入到队列中
setTimeout(() => {
  console.log(2) // 宏任务2的内容
}, 1000)

setTimeout(() => {
  console.log(4) // 宏任务3的内容
}, 0)

console.log(3) // 宏任务1的内容
```
宏任务和微任务混合：

```javascript
console.log(1)

// 宏任务
setTimeout(() => {
  console.log(2)
})

// 微任务
new Promise(function (resolve, reject) {
  console.log(5)
  resolve(3)
}).then(data => {
  console.log(data)
})

console.log(4)
```



# webpack概述

> webpack 是一个现代 javascript 应用程序的 **静态模块打包器 (module bundler)**
>
> 将来要学的 vue-cli 脚手架环境, 集成了 webpack, 所以才能对各类文件进行打包处理 

[webpack官网](https://webpack.js.org/)

## webpack 能做什么

webpack是一个 静态模块 打包器

1. 语法转换
   + less/sass转换成css
   + ES6转换成ES5
   + ...
2. html/css/js 代码压缩合并 (打包)
3. webpack可以在开发期间提供一个开发服务器

**项目一般先打包再上线**

# webpack的基本使用

https://webpack.docschina.org/

## webpack 打包演示

1. 建目录  dist    src/index.js

2. 初始化

   ```
   yarn init -y
   ```

3. 安装依赖包  ( -D: 将安装包作为开发阶段的依赖, 只在开发阶段使用, 实际上线不要的, 可以加 -D)

   dependencies  项目依赖, 实际上线, 也要用的包, 比如 jquery    yarn add jquery

   devDependencies 开发依赖, 实际上线, 不用这个包, 只在开发打包过程中用   -D

   ```
   yarn add webpack  webpack-cli  -D
   ```

4. 配置scripts 

   ```js
   scripts: {
   	"build": "webpack --config webpack.config.js"
   }
   ```

   `--config  webpack.config.js` 这个配置文件名为默认值, 不加也会默认找这个文件

5. 提供 webpack.config.js 

6. 运行打包命令

```
yarn build
```

小测试:

​	假定在index.js中导入一个  aa.js,  多个文件需要打包, wepack会打包成一个文件, 可以节约请求的次数

```js
import './js/aa.js'
console.log('这是main模块')
```



## npm中scripts的使用

在package.json文件中, 可以配置 scripts ...  配置自己的命令(脚本)

```
"scripts": {
	"aa": "yarn add jquery",
	"build": "webpack --config webpack.config.js"
}

npm run aa
npm run build
```

**运行方式:  npm  run xx**

特殊的命令:  start / stop  可以省略 run

```
npm run start  => npm start      =>  yarn start
npm run stop  => npm stop        =>  yarn stop
```

使用 yarn 直接不需要加 run  

```
npm run aa  =>  yarn aa
npm run build => yarn buil
```

## 配置webpack的入口和出口

webpack.config.js

```jsx
// 进行webpack详细的配置
// 导出webpack的配置信息
// 运行在node环境中
const path = require('path')
module.exports = {
  // webpack的入口，代表对谁进行打包  默认会找 src/index.js
  entry: './src/main.js',
  // webpack的出口 最终文件需要打包到哪儿去
  output: {
    // 默认main.js
    filename: 'bundle.js',
    // 配置打包到哪个路径 默认是dist 必须是绝对路径
    path: path.join(__dirname, 'lib')
  }
}
```

## **自动生成html** - `html-webpack-plugin` 插件 

https://www.webpackjs.com/plugins/html-webpack-plugin/

目前我们都是在 index.html 中手动引入打包后的资源，这种引入方式是有缺点的,  

将来上线需要把 index.html 和 dist 目录合到一起, 且目录引入也有问题, 需要改导入的路径

所以一般会用一个插件, 会自动将 public 中的 index 拷贝到 dist下, 并自动引入打包后的文件

  1. 下载

     ```
     yarn add html-webpack-plugin  -D
     ```

  2. **在`webpack.config.js`文件中，引入这个模块** :

     ```js
     // 引入自动生成 html 的插件
     const HtmlWebpackPlugin = require('html-webpack-plugin')
     ```

  3. 配置

     ```js
     plugins: [
       new HtmlWebpackPlugin({ template: './public/index.html' })
     ]
     ```

> **配置好了之后, public 目录的 index.html 就不需要引入打包后的文件了, 会自动被插件生成 html 引入**

`public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>

<div id="app">
  <!-- ul>li{我是第$个li}*10 -->
  <ul>
    <li>我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
    <li>我是第4个li</li>
    <li>我是第5个li</li>
    <li>我是第6个li</li>
    <li>我是第7个li</li>
    <li>我是第8个li</li>
    <li>我是第9个li</li>
  </ul>
</div>

<!-- 打包后的文件会被自动引入, 不需要手动引入了 -->
</body>
</html>
```

# webpack - loaders 的配置

webpack默认只认识 js 文件和 json文件, 但是webpack 可以使用 [loader](https://www.webpackjs.com/concepts/loaders) 来加载预处理文件, 允许webpack也可以打包 js之外的静态资源。

所以webpack如果要处理其他文件类型, **记得要先配置对应的 loader**

## webpack中处理 css 文件

**需求: 去掉小圆点, 新建 css 目录**

​	css/base.css

```css
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
```

​	main.js

```javascript
// 导入css样式
import './css/base.css'
```



1. 安装依赖

   ```
   yarn add style-loader css-loader -D
   ```

2. 配置

   ```js
   module: {
     // loader的规则
     rules: [
       {
         // 正则表达式，用于匹配所有的css文件
         test: /\.css$/,
         // 先用 css-loader 让webpack能够识别 css 文件的内容
         // 再用 style-loader 将样式, 以动态创建style标签的方式添加到页面中去
         use: [ "style-loader", "css-loader"]
       }
     ]
   },
   ```

**注意use配置项数组中的 各项顺序是不可以颠倒的，配置项优先级是从右向左。**

## webpack 中处理 less 文件

less/index.less

```less
ul {
  border: 1px solid #ccc;
  li {
    height: 30px;
    line-height: 30px;
  }
}
```

main.js

```javascript
// 导入less样式
import './less/index.less'
```





1. 下载依赖包

   注意: 解析less文件需要识别 less 语法, 所以除了 `less-loader`  需要额外下载 `less` 包  

   less-loader: 将less转换成 css

   ```
   yarn add less  less-loader  -D
   ```

2. 配置

   ```js
   module: {
     // loader的规则
     rules: [
       {
         test: /\.less$/,
         // less-loader 让webpack能够识别 less 文件的内容，并且把less自动转成css。
         // less-loader 依赖的 less包会自动把less转成css
         use: [ "style-loader", "css-loader", 'less-loader']
       }
     ]
   },
   ```

## webpack 中处理 scss 文件

scss/a.scss

```less
$color: pink;
ul {
  li {
    border-bottom: 1px solid $color;
  }
}
```

main.js

```javascript
// 导入scss样式
import './scss/a.scss'
```

## webpack 中处理图片 - url-loader

创建images文件夹存放一张图片

less/index.less

```less
ul {
  border: 1px solid #ccc;
  // 给ul加一个背景图片
  background-image: url(../images/1.gif);
  li {
    height: 30px;
    line-height: 30px;
  }
}
```



我们试了一下，在`css`中用到一下背景图片，结果就报错了，难道`webpack`连图片也认不出来吗？

没有错，的确认不出来, 此时需要转换图片的 loader, 来处理图片的问题,  主要用到 `url-loader`  和   `file-loader`

**注意: `url-loader` 中的部分功能要用到 `file-loader`,  要下载两个模块**

1. 下载依赖包

   ```
   yarn add url-loader file-loader -D
   ```

2. 配置loader

   ```javascript
{
     test: /\.(png|jpg|gif|jpeg)$/i,
     use: 'url-loader'
}
   ```

   

   图片转成 base64 字符串,  
   
   - 好处就是浏览器不用发请求了，直接可以读取
   - 坏处就是如果图片太大，再转`base64`就会让图片的体积增大 30% 左右, 得不偿失
   
   所以需要通过 options 配置选项进行配置 limit, 可以设置一个临界值, 大于这个值会整个文件直接打包到目录中, 得到是路径,
   
   如果小于这个值, 就会转成 base64, 节约请求的次数
   
   ```js
   {
     test: /\.(png|jpg|gif|jpeg)$/i,
     use: [
       {
         loader: 'url-loader',
         // 配置了limit, 超过8k, 不转, 
         // 不到8k, 转base64字符串
         options: {
           limit: 8 * 1024,
         },
       },
     ],
   }
   ```

## webpack 配置字体图标 - url-loader

创建fonts文件夹，存放字体文件。

main.js

```javascript
import './fonts/iconfont.css'
```

public/index.html

```html
<!--增加如下两行设置使用字体图标-->
<li><span class="iconfont icon-weixin"></span></li>
<li><span class="iconfont icon-qq"></span></li>
```



字体图标 和 图片的配置一致

``` js
// 处理字体图标的解析
{
  test: /\.(eot|svg|ttf|woff|woff2)$/,
  use: [
    {
      loader: 'url-loader',
      options: {
        limit: 8 * 1024
      }
    }
  ]
}
```

## webpack 使用 babel 处理高版本的 js 语法

https://www.babeljs.cn/

**webpack 默认仅内置了 模块化的 兼容性处理**   `import  export`

babel 的介绍 => 用于处理高版本 js语法 的兼容性

```javascript
const fn = () => {
  console.log('哈哈')
}
console.log(fn)
// 打包以后的js代码中依然是箭头函数，低版本的浏览器就不能执行了。我们希望能帮我们转换成低版本的语法，提供更好兼容性。
```

  1. 安装包

     ```
     yarn add -D babel-loader @babel/core @babel/preset-env
     ```

  2. 配置规则

     ```js
     module: {
       rules: [
         {
           test: /\.js$/,
           exclude: /(node_modules|bower_components)/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@babel/preset-env']
             }
           }
         }
       ]
     }
     ```

# webpack 开发服务器

每次修改代码, 都需要重新 yarn build 打包, 才能看到最新的效果, 且实际工作中, 打包 yarn build 非常费时 (30s - 60s) 之间

为什么费时? 

1. 构建依赖
2. 读取对应的文件, 才能加载  
3. 用对应的 loader 进行处理  
4. 将处理完的内容, 写入到 dist 目录  

可以起一个开发服务器,  在电脑内存中打包, 缓存一些已经打包过的内容, 只重新打包修改的文件, 最终运行加载在内存中

## webpack-dev-server自动刷新

> 注意：需要对webpack进行降级处理, 因为webpack已经升为5版本，但是webpack-dev-server还没有升级

```
yarn add webpack@4.43.0 webpack-cli@3.3.12 -D
yarn add html-webpack-plugin@4.5.2 -D
yarn add less-loader@7.3.0 -D
yarn add sass-loader@10.1.1 -D
```

1. 下载

```
yarn add webpack-dev-server -D
```

2. 配置scripts

```js
scripts: {
	"build": "webpack --config webpack.config.js",
	"dev": "webpack serve --config webpack.config.js"
}
```

## webpack-dev-server 的配置

```js
devServer: {
  port: 3000, // 端口号
  open: true // 自动打开浏览器
}
```

# source map 的说明 (了解) 

source map => 用于在浏览器中调错使用

## 生产环境遇到的问题

前端项目在投入生产(上线)环境之前，都需要对 JavaScript 源代码进行压缩混淆，从而减小文件的体积，

提高文件的加载效率。此时就不可避免的产生了另一个问题：

对压缩混淆之后的代码除错（debug）是一件极其困难的事情

- 变量被替换成没有任何语义的名称
- 空行和注释被剔除



## source map 介绍

Source Map 就是一个信息文件，里面储存着 **位置信息**。

也就是说: Source Map 文件中 **存储着** 压缩混淆后代码 对应的 **转换前的位置**。有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试。

### 开发环境的 source map

在`mode: development`开发环境下，webpack 默认启用了 Source Map 功能。

当程序运行出错时，可以直接在控制台提示错误行的位置，并定位到具体的源代码：

**但是, 默认的行数不对应, 加上如下配置即可解决行数不对应的问题**

```jsx
module.exports = {
  // 开发模式下不配置mode 默认就是mode: 'development'
  mode: 'development',
  // eval-source-map 开发模式下使用, 保证运行时的行数 和 源代码行数 一致
  devtool: 'eval-source-map',
  
  ...
}
```

### 生产环境的 source map

在生产环境下，如果省略了 devtool 选项，则最终生成的文件中不包含 Source Map。

这能够防止原始代码通过 Source Map 的形式暴露给别有所图之人。

1. 在生产环境下，如果只想 **定位报错的具体行数，且不想暴露源码**。 (推荐)

   此时可以将devtool 的值设置为  nosources-source-map

   ```jsx
   devtool: 'nosources-source-map',
   ```

2. 在生产环境下，如果想在定位报错行数的同时，展示具体报错的源码。

   此时可以将 devtool 的值设置为 source-map。

   ```jsx
   devtool: 'source-map',
   ```


source map:  打包后，会发现错误信息不准确，给我们开发带来了很大的影响。

devtool进行配置

​    eval-source-map：会显示具体的行号，以及打包前的源码

​	source-map：会显示具体的行号，以及打包前的源码，，  把这些信息生成到一个.map文件

   nosources-source-map： 依旧会生成map文件，但是不会显示具体的源码

# Vue基本概念 

## vue介绍

- Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式javascript框架**。  

### 渐进式的概念

渐进式：逐渐增强，可以在项目中使用vue的一部分功能，也可以使用vue的全家桶来管理整个项目。

vue: 全家桶 

### 框架的概念

- [我们所说的前端框架与库的区别？](https://zhuanlan.zhihu.com/p/26078359?group_id=830801800406917120)

**Library**

+ 代表：jQuery  art-template moment  axios

+ 库，本质上是一些函数的集合。每次调用函数，实现一个特定的功能   工具箱

- 使用库的时候，把库当成工具使用，需要自己控制代码的执行逻辑。
- 写作文
- 使用工具手动榨果汁：![榨果汁工具库](./images/榨果汁工具库.png)

**Framework**

+ 代表：vue、angular、react、bootstrap

+ 框架，是一套完整的解决方案

- 使用框架的时候，框架实现了大部分的功能，我们只需要按照框架的规则写代码
- 完形填空
- 使用榨汁机自动榨果汁：![榨果汁工具库](./images/榨汁机.png)

**库和框架的区别**

+ 使用库的时候，很自由，只要调用库提供的各种各样的方法就行，也可以不用其他的一些方法
+ 使用框架的时候，需要按照框架的规则写代码，限制会非常多，但同时框架的功能也很强大，可以极大的提升开发的效率。

## vue是MVVM的框架

+ MVVM，一种软件架构模式，决定了写代码的方式。
  + M：model数据模型(ajax获取到的数据)	
  + V：view视图（页面）
  + VM：ViewModel 视图模型

- MVVM通过`数据双向绑定`让数据自动地双向同步  **不在需要操作DOM**
  - V（修改视图） -> M（数据自动同步）
  - M（修改数据） -> V（视图自动同步）

**1. 在vue中，不推荐直接手动操作DOM！！！**  

**2. 在vue中，通过数据驱动视图，不要在想着怎么操作DOM，而是想着如何操作数据！！**

## vue组件化思想

模块化：一个独立的js文件就是一个模块  

组件化：一个组件会包含（HTML+CSS+JS） 把一个完整的页面拆分成多个组件构成。

组件 (Component) 是 Vue.js 最强大的功能之一。

在vue中都是组件化开发的，组件化开发就是把一个完整的页面分割成一个一个的小组件。

组件的优点：

+ 容易维护
+ 复用

## 开发vue的方式

开发vue有两种方式

+ 传统开发模式：基于html/css/js文件开发vue
+ 工程化开发方式：在webpack环境中开发vue，这是最推荐的方式。现代化的项目也都是在webpack环境下进行开发的。
  + 学习
  + 项目

# vue-cli的使用

> `vue-cli`也叫vue脚手架,`vue-cli`是vue官方提供的一个全局命令工具，这个命令可以帮助我们快速的创建一个vue项目的基础架子。

+ 开箱即用
+ 零配置
+ webpack、babel

## 基本使用

官网： https://cli.vuejs.org/zh/guide/

+ 全局安装命令

```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

+ 查看版本`vue`

```js
vue --version
```

+ 初始化一个vue项目

```js
vue create 项目名
```

+ 启动项目

```javascript
// 当前项目的根目录运行命令
yarn serve
```



## 如何覆盖webpack配置

> 注意：我们在项目无法找到webpack.config.js文件，因为vue把它隐藏。

如果需要覆盖webpack的配置，可以创建一个vue.config.js文件，覆盖webpack配置文件

```jsx
/* 覆盖webpack的配置 */
module.exports = {
  devServer: {
    open: true,
    port: 3000
  }
}
```

## vue单文件组件的说明

app.vue

```vue
<template>
  <div>
    <!-- 页面结构 -->
    <p>123</p>
    <a href="#">哈哈哈</a>
  </div>
</template>

<script>
console.log('123')
</script>

<style lang="less">
/* lang: less scss 默认是css */
// vue-cli默认帮助我们配置好了less 但是并没有安装依赖包,因为vue-cli不确定我们项目到底是不是要是用 less 还是sass 或是css。
 yarn add less-loader less -D
// yarn add less-loader@7.3.0 less -D
@color: red;
p {
  color: @color;
}

</style>
```



一个`.vue`文件就是一个组件,后续开发vue，所有的功能都是基于组件实现。

一个单文件组件由三部分构成

+ template(必须)  影响组件渲染的结构  html
  + 只能有一个根元素
+ script                     逻辑   js
+ style                       样式   css less scss
  + style用于提供组件的样式，默认只能用css
  + 可以通过`lang="less"`开启less的功能，需要安装依赖包

# vue的插值表达式

## vue通过data提供数据

> vue中通过template可以提供模板，但是这样的数据是写死的。

vue可以通过data提供数据，注意：`data必须是一个函数，并且返回一个对象`

```jsx
<script>
export default {
  data () {
    return {
      money: 100,
      msg: 'hello'
    }
  }    
}
</script>
```

## 通过插值表达式显示数据

插值表达式, 小胡子语法` {{  }}`

1. 作用:  使用 data 中的数据渲染视图（模板）

2. 基本语法, 支持三元运算符

   ```jsx
   {{ msg }}
   {{ obj.name }}
   {{ msg.toUpperCase() }}
   {{ obj.age > 18 ? '成年' : '未成年' }}
   ```

3. vue中插值表达式的注意点

   (1)  使用的数据在 data 中要存在

   ```jsx
   <h1>{{ gaga }}</h1>
   ```

   (2)  能使用表达式, 但是不能使用 if  for

   ```jsx
   <h1>{{ if (obj.age > 18 ) { }   }}</h1>
   ```

   (3)  不能在标签属性中使用

   ```jsx
   <h1 id="box" class="box" title="{{ msg }}"></h1>
   ```

Q&A：

	1.  vue中数据配置在哪里，如何配置？
 	2.  插值表达式功能是什么？
 	3.  插值表达式中能不能写if语句？
 	4.  插值表达式能不能用在标签的属性上？

# vue指令



**vue指令, 实质上就是特殊的 html 标签属性, 特点:  v- 开头**

每个 v- 开头的指令, 都有着自己独立的功能, 将来vue解析时, 会根据不同的指令提供不同的功能

## v-bind指令

- 描述：插值表达式不能用在html的属性上，如果想要动态的设置html元素的属性，需要使用v-bind指令
- 作用：动态的设置html的属性
- 语法：`v-bind:title="msg"`
- 简写：`:title="msg"`

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

## v-on指令

### 基本使用

+ 最基本的语法

  + `<button v-on:事件名="事件函数">按钮</button>`，需要在methods中提供事件处理函数

  ```jsx
  <button v-on:click="fn">搬砖</button>
  <button v-on:click="fn1">卖肾</button>
  
    // 提供方法
    methods: {
      fn () {
        console.log('你好啊')
        // console.log(this)
        this.money++
      },
      fn1 () {
        this.money += 10000
      },
    }
  ```

+ 需要传递参数

  + `<button v-on:事件名="事件函数(参数)">按钮</button>`，需要在methods中提供事件函数，接受参数

  ```jsx
  <button v-on:click="addMoney(1)">搬砖</button>
  <button v-on:click="addMoney(10000)">卖肾</button>
  
  methods: {
    addMoney (money) {
      this.money += money
    }
  }
  ```

+ 如果事件的逻辑足够简单，可以不提供函数

```jsx
<button v-on:click="money++">搬砖</button>
<button v-on:click="money += 10000">卖肾</button>
```

### vue中获取事件对象(了解)

需求: 默认a标签点击会跳走,  希望阻止默认的跳转, 阻止默认行为  e.preventDefault()

vue中获取事件对象

(1) 没有传参, 通过形参接收 e

(2) 传参了, 通过$event指代事件对象 e

```jsx
<template>
  <div id="app">
    <a @click="fn" href="http://www.baidu.com">去百度</a>
    <a @click="fn2(100, $event)" href="http://www.baidu.com">去百度2</a>
  </div>
</template>

<script>
export default {
  methods: {
    fn(e) {
      e.preventDefault()
    },
    fn2(num, e) {
      e.preventDefault()
    }
  }
}
</script>
```

### v-on 事件修饰符

- vue中提供的事件修饰符

  .prevent 阻止默认行为

  .stop 阻止冒泡

```html
<div id="app">
  <a @click.prevent="fn" href="http://www.baidu.com">去百度</a>
</div>
```

### 按键修饰符

需求: 用户输入内容, 回车, 打印输入的内容

在监听键盘事件时，我们经常需要判断详细的按键。此时，可以为键盘相关的事件添加按键修饰符

- @keyup.enter  回车

- @keyup.esc  返回

```html
<div id="app">
  <input type="text" @keyup="fn"> <hr>
  <input type="text" @keyup.enter="fn2">
</div>

```

## v-if 和 v-show

### 基本使用

v-show 和 v-if 功能: 控制盒子的显示隐藏

1. v-show

   语法:  v-show="布尔值"    (true显示, false隐藏)

   原理:  实质是在控制元素的 css 样式,  `display: none;`

2. v-if

   语法: v-if="布尔值"   (true显示, false隐藏)

   原理:  实质是在动态的创建 或者 删除元素节点

应用场景: 

- 如果是频繁的切换显示隐藏, 用 v-show

  v-if, 频繁切换会大量的创建和删除元素, 消耗性能

- 如果是不用频繁切换, 要么显示, 要么隐藏的情况, 适合于用 v-if

  v-if 是惰性的, 如果初始值为 false, 那么这些元素就直接不创建了, 节省一些初始渲染开销

```html
<template>
  <div id="app">
    <h1 v-show="isShow">v-show盒子-{{ msg }}</h1>
    <h1 v-if="isShow">v-if盒子-{{ msg }}</h1>
  </div>
</template>
```

### v-else 和 v-else-if

```html
<div id="app">
  <h1 v-if="isLogin">尊敬的超级vip, 你好</h1>
  <h1 v-else>你谁呀, 赶紧登陆~</h1>

  <hr>
  
  <h1 v-if="age >= 60">60岁以上, 广场舞</h1>
  <h1 v-else-if="age >= 30">30岁以上, 搓麻将</h1>
  <h1 v-else-if="age >= 20">20岁以上, 蹦迪</h1>
  <h1 v-else>20岁以下, 唱跳rap篮球</h1>
</div>

```

## v-model

### 基本使用

**作用: 给表单元素使用, 双向数据绑定 ** 

1. 数据变化了, 视图会跟着变

2. 视图变化了, 数据要跟着变

   输入框内容变化了(监听用户的输入, 监听input事件), 数据要跟着变

语法: v-model='值'

```jsx
<input type="text" v-model="msg">
```

### v-model 处理其他表单元素

**v-model 会忽略掉表单元素原本的value, checked等初始值**

textarea, select, checkbox

```vue
<template>
  <div>
    <h1>v-model操作其他表单元素</h1>
    <div>
      姓名: <input type="text" v-model="username">
    </div>
    <div>
      描述: <textarea cols="30" rows="10" v-model="desc"></textarea>
    </div>
    <div>
      是否单身：<input type="checkbox" v-model="isDog">
    </div>
    <div>
      是否单身：
      <input type="radio" name="a" :value="true" v-model="isDog">是
      <input type="radio" name="a" :value="false" v-model="isDog">否
    </div>
    <div>
      城市：<select v-model="city">
        <option :value="1">北京</option>
        <option :value="2">上海</option>
        <option :value="3">广州</option>
        <option :value="4">深圳</option>
      </select>
    </div>
    <div>
      
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      username: '张三',
      desc: '',
      isDog: true,
      city: 2
    }
  }
}
</script>

<style>

</style>
```



### v-model 修饰符

- number

  如果想自动将用户的输入值, 用parseFloat转成数字类型, ，可以给 `v-model` 添加 `number` 修饰符：

  ```html
  <input v-model.number="age" type="number">
  ```

  如果这个值如果这个值无法转数字，则会返回原始的值。

- trim

  如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

  ```html
  <input v-model.trim="msg">
  ```

- lazy

  在`change`时而非`input`时更新，可以给 `v-model` 添加 `lazy` 修饰符：

  ```html
  <input v-model.lazy="msg">
  ```



## v-text 和 v-html

### v-text指令

- 解释：更新元素的 `textContent/innerText`。如果要更新部分的 `textContent` ，需要使用 `{{ Mustache }}` 插值。 

```html
<h1 v-text="msg"></h1>
```

### v-html指令

- 解释：更新DOM对象的` innerHTML`,html标签会生效

```html
<h1 v-html="msg"></h1>
```

**在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 [XSS 攻击](https://baike.baidu.com/item/XSS%E6%94%BB%E5%87%BB/954065?fr=aladdin)。只在可信内容上使用 `v-html`，**永不**用在用户提交的内容上。** 

## v-for 

### 基本使用

 v-for 作用: 遍历对象和数组

1. 遍历数组 (常用)

```jsx
v-for="item in 数组名"  item每一项
v-for="(item, index) in 数组名"  item每一项 index下标

注意：item和index不是定死的，可以是任意的名字，但是需要注意 第一项是值  第二项是下标
```

2. 遍历对象 (一般不用)

```jsx
<!--
  v-for也可以遍历对象（不常用）
  v-for="(值, 键) in 对象"
-->
<ul>
  <li v-for="value in user" :key="value">{{value}}</li>
</ul>
<ul>
  <li v-for="(value, key) in user" :key="key">{{value}} ---{{key}}</li>
</ul>
```

3. 遍历数字

```jsx
<!-- 
  遍历数字
  语法： v-for="(item, index) in 数字"
  作用：遍历具体的次数 item从1开始  index下标从0开始的
-->
<ul>
  <li v-for="(item, index) in 10" :key="item">{{item}} ---{{index}}</li>
</ul>
```

### v-for 的key的说明

首先不加key属性也是可以运行的，报错是因为eslint给的提示。如下配置可以允许不加key属性也能正常运行：

vue.config.js：(为了给大家演示不加key也能正常显示，但是正常开发中我们不要这么配置)

```javascript
module.exports = {
  // 保存代码的时候，不校验代码的格式
  lintOnSave: false
}
```



总结: 一般为了优化渲染的性能, 可以在遍历列表时, 加上一个 key属性, key一般指定成 id 

### 虚拟DOM 和 diff算法

#### 虚拟Dom

 - vue的DOM渲染过程：template => 虚拟DOM => 真实DOM对象

 - 虚拟DOM: 实质上就是一个普通的js对象，会包含一个真实DOM对象渲染必须的一些属性

   ```javascript
   const p = {
   
     type: 'p',
   
     attributes: [{id: box}],
   
     children: '我是内容'
   
    }
   ```

   

 - 采用虚拟DOM目的是提高更新DOM节点更新时候的效率，而不是为了提升初次渲染时候的效率。

   	- 当template内容发生变化，会生成新的虚拟DOM，通过diff算法和原虚拟DOM进行比较找出差异DOM，然后在页面上针对有变化的部分进行更新。

#### diff算法

​	参考react：https://react.docschina.org/docs/reconciliation.html

1. 如果根元素发生了改变，DOM不复用，直接销毁重建

   ```html
   <div>123</div>
   <p>456</p>
   <!--
   {type: 'div', children: '123'}
   {type: 'p', children: '456'}
   -->
   ```



​	2. 如果根元素不变，只是属性或者内容变化，，会复用DOM结构，更新对应的内容
   ```html

<div className="before" title="stuff">123</div>
<div className="after" title="stuff">456</div>
<!--
{type: 'div', children: '123' class= "before"}
{type: 'div', children: '456' class= "after"}
-->
   ```

3. key属性作用
   1. 如果同一节点下同类型的子元素被更新，如果虚拟DOM有key选项，diff算法会以key选项为唯一标识。

```javascript

/*
      {
        type: 'ul',
        children: [
          { type: 'li', children: 'first', key: 1 },
          { type: 'li', children: 'second', key: 2 },
        ]
      }

      {
        type: 'ul',
        children: [
          { type: 'li', children: 'third', key: 3 },
          { type: 'li', children: 'first', key: 1 },
          { type: 'li', children: 'second', key: 2 },
        ]
      }
*/
```

Q&A：

​	1、我们以后工作中使用v-for指令的时候一定要把数据中的id绑定给哪个属性？

## 样式处理

###  v-bind 对于class的增强

v-bind 对于类名操作的增强, 注意点, :class 不会影响到原来的 class 属性

:class="对象/数组"

```jsx
<template>
  <div>
    <!-- 
      v-bind： 作用：设置动态属性
      v-bind针对 class和style 进行增强
      允许使用对象或者数组
        对象：如果键值对的值为true，那么就有这个，否则没有这个类
        数组：数组中所有的类都会添加到盒子上
    -->
    <!-- <div class="box" :class="isRed ? 'red': ''">123</div> -->
    <!-- <div class="box" :class="{red: isRed, pink: isPink}">123</div> -->
    <div class="box" :class="arr">123</div>
  </div>
</template>
export default {
  data () {
    return {
      isRed: true,
      isPink: true,
      arr: ['red', 'pink']
    }
  },
}
</script>

<style>
.box {
  width: 100px;
  height: 100px;
  border: 1px solid #ccc;
}
.red {
  color: red;
}
.pink {
  background-color: pink;
}
</style>

```

### v-bind对于style 的增强

```jsx
<template>
  <div>
    <!-- 
      :style也可以使用对象或者数组
     -->
    <div class="box" :style="{color:color, backgroundColor: bg }">123</div>
  </div>
</template>
<script>

export default {
  data () {
    return {
      color: 'red',
      bg: 'yellow'
    }
  },
}
</script>

<style>
.box {
  width: 100px;
  height: 100px;
  border: 1px solid #ccc;
}

</style>
```

# 过滤器

## 过滤器的基本使用

`过滤器（Filters）`是 vue 为开发者提供的功能，常用于文本的格式化。

过滤器可以用在两个地方：`插值表达式`和 `v-bind 属性绑定`。

过滤器由“管道符”`|`进行调用，示例代码如下：

```jsx
<!-- 通过过滤器对message进行过滤 -->
<p>{{message | 过滤器}}</p>

<!-- 通过过滤器对title进行过滤 -->
<div :title="title | 过滤器"></div>
```

## 定义过滤器

`局部过滤器`: 组件私有的过滤器，可以在 filters 节点中定义过滤器，该过滤器只能在当前组件中调用

```jsx
export default {
  /* 
    filters: {
      过滤器名字 (过滤的值) {
        return 处理后的值
      }
    }
  */
  filters: {
    upper (input) {
      return input.slice(0, 1).toUpperCase() + input.slice(1)
    }
  }
}
```

`全局过滤器`： 如果希望在多个 vue 组件之间共享过滤器，可以在`main.js`中通过`Vue.filter()`方法定义全局过滤器

```jsx
// 参数1： 过滤器名称
// 参数2： 过滤器函数，处理过滤逻辑
Vue.filter('upper', function (input) {
  return input.slice(0, 1).toUpperCase() + input.slice(1)
})
```

Q&A：

​	1、什么场景下要使用过滤器？

​	2、 哪种过滤器要定义成全局过滤器？

## 调用多个过滤器

> 过滤器可以串联地进行调用

```jsx
<template>
  <!-- 把msg的值，交给filterA进行处理 -->
  <!-- 把filterA处理的结果，再交给filterB进行处理 -->
  <!-- 最终把filterB处理，把最终的值渲染到页面 -->
  <div>{{msg | filterA | filterB}}</div>
</template>
```

案例：让单词首字母大写，且最多显示10个字符

```vue

<template>
  <div>
    <p>{{msg | a}}</p>
    <p>{{msg | b}}</p>
    <p>{{msg | a | b}}</p>
    <p>{{msg | a | b(10)}}</p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'absdfsdfdscde'
    }
  },
  filters: {
    a (input) {
      return input.slice(0, 1).toUpperCase() + input.slice(1)
    },
    b (input, length = 5) {
      return input.length > length ? input.slice(0, length) + '...' : input
    }
  }
}
</script>

<style>

</style>
```

## 过滤器传参

在使用过滤器的时候，可以额外传递参数

```jsx
// 使用过滤器的时候可以传递额外的参数
<p>{{msg | filterA(arg1, arg2)}}</p>

// 定义过滤器的时候，需要接收额外的参数
filters: {
  // 建议给过滤器的额外参数提供默认值
  filterA (input, arg1 = 0, arg2 = 1) {
    
  }
}
```

# 计算属性

## 基本使用

需求：翻转字符串案例

> 计算属性是一个 function，这个 function 的返回值就是计算属性最终的值。

> 1. 计算属性必须定义在 computed 节点中

> 2. 计算属性必须是一个 function,计算属性必须有返回值

> 3. 计算属性不能被当作方法调用,当成属性来用

什么时候会用到计算属性？？？？

1. 在模板中书写大量的js逻辑，，，模板会变得不好维护，把这段逻辑写到计算属性中
2. 我们需要一个属性，但是这个属性不是立马得到的，需要通过逻辑运算才能得到。

定义计算属性

```jsx
// 组件的数据： 需要计算的属性
computed: {
  reverseMsg () {
    return this.msg.split('').reverse().join('')
  }
}
```

使用计算属性

```jsx
<p>{{ reverseMsg }}</p>
```

## 品牌管理案例-计算属性处理总价钱

+ 在computed中提供计算属性

```jsx
computed: {
  total: function () {
    // 把this.list中所有的价格加起来
    let total = 0
    this.list.forEach(item => total += +item.price)
    return total
  }
},
```

+ 在模板中直接渲染计算属性

```jsx
<td colspan="2">
  总价钱：{{ total }} 万
</td>
```

+ 平均价钱

```jsx
computed: {
  total: function () {
    // 把this.list中所有的价格加起来
    let total = 0
    this.list.forEach(item => total += +item.price)
    return total
  },
  avg () {
    return (this.total / this.list.length).toFixed(2)
  }
},
```

## 计算属性的缓存特性

计算属性： 缓存

计算属性只要计算了一次，就会把结果缓存起来，以后多次使用计算属性，直接使用缓存的结果，只会计算一次。

计算属性依赖的属性一旦发生了改变，计算属性会重新计算一次，并且缓存

```jsx
// 计算属性只要计算了一次，就会把结果缓存起来，以后多次使用计算属性，直接使用缓存的结果，只会计算一次。
// 计算属性依赖的属性一旦发生了改变，计算属性会重新计算一次，并且缓存
export default {
  data () {
    return {
      msg: 'hello'
    }
  },
  computed: {
    reverseMsg() {
      console.log('我执行了')
      return this.msg.split('').reverse().join('')
    }
  }
}
```

## 计算属性的完整写法

```jsx
// 1. 计算属性默认情况下只能获取，不能修改。
// 2. 计算属性的完整写法
/* 
  computed: {
    full() {},
    full: {
      get() {
        return this.first + ' ' + this.last
      },
      set(value) {

      }
    }
  }
*/
computed: {
  full: {
    get () {
      return this.first + ' ' + this.last
    },
    set (value) {
      console.log('修改了计算属性', value)
      this.first = value.split(' ')[0]
      this.last = value.split(' ')[1]
    }
  }
}
```

# 属性监听

## 基本使用

当需要监听某个数据是否发生改变，就要用到watch

```jsx
/* 
  watch: {
    // 只要属性发生了改变，这个函数就会执行
    属性: function () {

    }
  }
*/
watch: {
  // 参数1： value    变化后的值
  // 参数2： oldValue 变化前的值
  msg (value, oldValue) {
    console.log('你变了', value, oldValue)
  }
}
```

## 复杂类型的监听-监听的完整写法

> 如果监听的是复杂数据类型，需要深度监听，需要指定deep为true,需要用到监听的完整的写法

```jsx
// 1. 默认情况下，watch只能监听到简单类型的数据变化,如果监听的是复杂类型，只会监听地址是否发生改变，不会监听对象内部属性的变化。
// 2. 需要使用监听的完整写法 是一个对象
watch: {
  // friend (value) {
  //   console.log('你变了', value)
  // }
  friend: {
    // handler 数据发生变化，需要执行的处理程序
    // deep: true  如果true,代表深度监听，不仅会监听地址的变化，还会监听对象内部属性的变化
    // immediate: 立即 立刻  是否立即监听 默认是false  如果是true,代表页面一加载，会先执行一次处理程序
    handler (value) {
      console.log('你变了', value)
    },
    deep: true,
    immediate: true
  }
},
```

## 品牌管理-监听数据进行缓存

+ 监听list的变化

```jsx
watch: {
  // 深度监听
  list: {
    deep: true,
    handler (value) {
      console.log('你变了', value)
      // 把value保存到localStorage中
      // localStorage只能储存字符串类型
      localStorage.setItem('brand-list', JSON.stringify(value))
    }
  }
}
```

+ 获取list数据的时候不能写死，从localStorage中获取

```jsx
data() {
  return {
    list: JSON.parse(localStorage.getItem('brand-list')) || [],
    brand: '',
    price: ''
  }
},
```



# 组件化开发

## 什么是组件化开发

**组件化开发** 指的是：根据封装的思想，把页面上 `可重用的部分` 封装为 `组件`，从而方便项目的 开发 和 维护。

**一个页面， 可以拆分成一个个组件，一个组件就是一个整体, 每个组件可以有自己独立的 结构 样式 和 行为**

前端组件化开发的好处主要体现在以下两方面：

- 提高了前端代码的**复用性和灵活性**  

- 提升了开发效率和后期的**可维护性**

  

vue 是一个完全支持组件化开发的框架。vue 中规定组件的后缀名是 `.vue`。

## 组件的注册

刚才我们创建使用的是 App.vue 根组件, 这个比较特殊, 是最大的一个根组件

而App.vue根组件内, 还可以写入一些小组件, 而这些组件, 要使用, 就需要先注册!

**注册组件有两种注册方式**:  分为“全局注册”和“局部注册”两种

- 被全局注册的组件，可以在任意的组件模板范围中使用 通过`Vue.component()`
- 被局部注册的组件，只能在当前注册的组件模板范围内使用 通过`components`

### 局部注册

+ 把独立的组件封装一个`.vue文件中`，推荐放到`components`文件夹

```jsx
components
  -- HmHeader.vue
  -- HmContent.vue
  -- HmFooter.vue
```

+ 通过组件的`components`配置 局部注册组件

```jsx
import HmHeader from './components/HmHeader'
import HmContent from './components/HmContent'
import HmFooter from './components/HmFooter'

export default {
  // data methods filters computed watch
  components: {
    // 组件名: 组件
    // 组件名：注意，不能和html内置的标签重名
    // 使用的时候：直接通过组件名去使用
    // HmHeader  HmHeader  hm-header
    HmHeader,
    HmContent,
    HmFooter
  }
}
```

==注意点：注册的组件的名字不能和HTML内置的标签重名==

+ 可以在模板中使用组件，，，，使用组件和使用html的标签是一样的，，，可以多次使用

```jsx
<template>
  <div>
    <!-- 组件注册好了，就跟使用html标签一样了 -->
    <hm-header></hm-header>
    <hm-content></hm-content>
    <hm-footer></hm-footer>
  </div>
</template>
```

==局部注册的组件只能在当前组件中使用==

## 全局注册组件

+ 在`components`文件夹中创建一些新的组件

```jsx
components
  -- HmHeader.vue
  -- HmContent.vue
  -- HmFooter.vue
```

+ 在`main.js`中通过`Vue.component()`全局注册组件

```jsx
import HmHeader from './components/HmHeader'
import HmContent from './components/HmContent'
import HmFooter from './components/HmFooter'

// 全局注册
// Vue.component(名字, 组件)
Vue.component('HmHeader', HmHeader)
Vue.component('HmContent', HmContent)
Vue.component('HmFooter', HmFooter)
```

+ 使用

```jsx
<template>
  <div>
    <!-- 组件注册好了，就跟使用html标签一样了 -->
    <hm-header></hm-header>
    <hm-content></hm-content>
    <hm-footer></hm-footer>
  </div>
</template>
```

==注意：全局注册的组件 可以在任意的组件中去使用==

### 组件名的大小写

在进行组件的注册时，定义组件名的方式有两种：

- 注册使用短横线命名法，例如 hm-header 和 hm-main

  ```js
  Vue.component('hm-button', HmButton)
  ```

  使用时 `<hm-button> </hm-button>`

- 注册使用大驼峰命名法，例如 HmHeader 和 HmMain

  ```jsx
  Vue.component('HmButton', HmButton)
  ```

  使用时 `<HmButton> </HmButton>` 和 `<hm-button> </hm-button>`  都可以

推荐定义组件名时, 用大驼峰命名法, 更加方便

全局注册

```jsx
Vue.component('HmButton', HmButton)
```

局部注册:

```jsx
components: {
  HmHeader,
  HmMain,
  HmFooter
}
```

使用时, 推荐遵循html5规范, 小写横杠隔开

```jsx
<hm-header></hm-header>
<hm-main></hm-main>
<hm-footer></hm-footer>
```

### 通过 name 注册组件 (了解)

在注册组件期间，除了可以直接提供组件的注册名称之外，还可以把组件的 name 属性作为注册后组件的名称。

- name会影响到开发者工具显示组件时候的名字
- 注册组件的时候可以使用name属性作为组件名

组件内容:

```jsx
<template>
  <button>按钮组件</button>
</template>

<script>
export default {
  name: 'HmButton'
}
</script>

<style lang="less">
button {
  width: 80px;
  height: 50px;
  border-radius: 5px;
  background-color: pink;
}
</style>
```

进行注册:

```jsx
import HmButton from './components/hm-button.vue'
Vue.component(HmButton.name, HmButton)  // 等价于 app.component('HmButton', HmButton)
```

## 组件的样式冲突  `scoped`

默认情况下，写在组件中的样式会`全局生效`，因此很容易造成多个组件之间的样式冲突问题。

组件样式默认会作用到全局, 就会影响到整个 index.html 中的 dom 元素

- `全局样式`: 默认组件中的样式会作用到全局

- `局部样式`: 可以给组件加上 scoped 属性, 可以让样式只作用于当前组件

```jsx
<style lang="less" scoped>
div {
  background-color: pink;
}
</style>
```

原理:

1. 添加scoped后, 会给当前组件中所有元素, 添加上一个自定义属性

2. 添加scoped后,  每个style样式, 也会加上对应的属性选择器


最终效果: 必须是当前组件的元素, 才会有这个自定义属性, 才会被这个样式作用到

# 组件通信

每个组件都有自己的数据, 提供在data中, 每个组件的数据是独立的, 组件数据无法互相直接访问 (合理的)

但是如果需要跨组件访问数据, 就需要用到组件通信

组件通信的方式有很多: 现在先关注两种,  父传子  子传父

## 组件通信 - 父传子 props 传值

语法:

1. 父组件通过给子组件加属性传值

```jsx
<my-product price="100" title="不错" :info="msg"></my-product>
```

2. 子组件中, 通过props属性接收

```js
props: ['price', 'title', 'info']
```

Q&A：

​	1、什么场景需要父传子？

**需求: 封装一个商品组件 my-product**

`my-product.vue`

```vue
<template>
  <div class="my-product">
    <h3>标题: {{ title }}</h3>
    <p>价格: {{ price }}元</p>
    <p>{{ info }}</p>
  </div>
</template>

<script>
export default {
  props: ['title', 'price', 'info']
}
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```



## v-for 遍历展示组件练习

**需求: 遍历展示商品列表**

假定, 发送请求回来的商品数据, 

```jsx
list: [
  { id: 1, proname: '超级好吃的棒棒糖', proprice: 18.8 },
  { id: 2, proname: '超级好吃的大鸡腿', proprice: 34.2 },
  { id: 3, proname: '超级无敌的冰激凌', proprice: 14.2 }
]
```

v-for 遍历展示

```jsx
<template>
  <div class="container">
    <h3>我是app组件的内容</h3>
    <my-product 
      v-for="item in list" :key="item.id" 
      :price="item.proprice" 
      :title="item.proname" 
      :info="msg">
    </my-product>
  </div>
</template>
```

## 单向数据流

```jsx
/* 
  在vue中需要遵循单向数据流原则
  1. 父组件的数据发生了改变，子组件会自动跟着变
  2. 子组件不能直接修改父组件传递过来的props  props是只读的

*/
```

==如果父组件传给子组件的是一个对象，子组件修改对象的属性，是不会报错的，，，，也应该避免==

## 组件通信 - 子传父

**需求: 砍价**

1. 子组件可以通过 `this.$emit('事件名', 参数1, 参数2, ...)` 触发事件的同时传参的

   ```jsx
   this.$emit('sayPrice', obj.id, 2)
   ```

2. 父组件给子组件注册一个自定义事件

   ```jsx
   <my-product 
     ...
     @sayPrice="sayPrice">
   </my-product>
   ```

   父组件并提供对应的函数接收参数

   ```jsx
   methods: {
     sayPrice (id, price) {
       console.log(id, price)
       const product = this.list.find(item => item.id === id)
       product.proprice -= price
     }
   },
   ```



## props 校验

**props 是父传子, 传递给子组件的数据, 为了提高 子组件被使用时 的稳定性, 可以进行props校验**, 验证传递的数据是否符合要求

默认的数组形式, 不会进行校验, 如果希望校验, 需要提供对象形式的 props

```jsx
props: []
props: {
	...
}
```

props 提供了多种数据验证方案，例如：

- 基础的类型检查  Number
- 多个可能的类型 [String, Number]
- 必填项校验   required: true
- 默认值 default: 100
- 自定义验证函数

官网语法: [地址](https://cn.vuejs.org/v2/guide/components-props.html#Prop-%E9%AA%8C%E8%AF%81)

```js
{
  props: {
    // 基础的类型检查
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // -------------------------------------------------------------------------
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
}
```

## 数据持久化功能

- 配置侦听器监听数据list

- ```javascript
  watch: {
      list: {
        deep: true,
        handler (value) {
          // 把最新的值保存起来
          localStorage.setItem('todos', JSON.stringify(value))
        }
      }
    },
  ```

- data中加载数据时候配置读取localstorage

- ```javascript
  
  list: JSON.parse(localStorage.getItem('todos')) || [],
  ```

- 

## 任务的筛选

+ 在父组件提供一个数据condition

```jsx
// 显示的条件
// all | active | completed
// 保存点击的是哪个按钮
condition: 'all'
```

+ 在父组件中提供一个showList的计算属性，传递给Main组件

```jsx
computed: {
  showList () {
    if (this.condition === 'active') {
      return this.list.filter(item => !item.isDone)
    } else if (this.condition === 'completed') {
      return this.list.filter(item => item.isDone)
    } else {
      return this.list
    }
  }
},
  
<todo-main 
  :list="showList" 
  @del="delFn" 
  @change="changeFn" 
  @checkAll="checkAll"
></todo-main>
```



+ 点击按钮的时候，需要子传父，去修改 condition

```jsx
<li>
  <a 
    href="#/" 
    @click.prevent="$emit('changeCondition', 'all')"
  >All</a>
</li>
<li>
  <a 
    href="#/active" 
    @click.prevent="$emit('changeCondition', 'active')">Active</a>
</li>
<li>
  <a 
    href="#/completed" 
    @click.prevent="$emit('changeCondition', 'completed')"
  >Completed</a>
</li>
```

+ 父组件接收条件并且修改

```jsx
<todo-footer :condition="condition" :list="list" @clear="clearFn" @changeCondition="changeCondition"></todo-footer>

changeCondition (condition) {
  this.condition = condition
}
```

+ 控制selected类名，，，，父组件把condition传给子组件

```jsx
<todo-footer :condition="condition" :list="list" @clear="clearFn" @changeCondition="changeCondition"></todo-footer>

props: ['list', 'condition'],
  
<li>
  <a 
    :class="{selected: condition === 'all'}" 
    href="#/" 
    @click.prevent="$emit('changeCondition', 'all')"
  >All</a>
</li>
<li>
  <a 
  :class="{selected: condition === 'active'}" 
    href="#/active" 
    @click.prevent="$emit('changeCondition', 'active')">Active</a>
</li>
<li>
  <a 
    href="#/completed" 
    :class="{selected: condition === 'completed'}" 
    @click.prevent="$emit('changeCondition', 'completed')"
  >Completed</a>
</li>
```

# 通用组件通讯方案-event-bus

## 核心语法和原理

如果两个组件的关系非常的复杂，那么通过父子组件通讯是非常麻烦的。这时候可以使用通用的组件通讯方案：事件总线（event-bus)

核心语法

```jsx
1. 创建事件总线   main.js
const bus = new Vue()
// 把bus挂载到了Vue的原型上, 保证所有的组件都能通过 this.bus访问到事件总线
Vue.prototype.bus = bus

2. 订阅事件 
bus.$on('事件名', 事件回调函数)
bus.$on('send', msg => {
  
})
// 1. 在created中订阅
// 2. 回调函数需要写成箭头函数

3. 发布事件
bus.$emit('事件名', 额外参数)
bus.$emit('send', 'hello')
```

## event-bus使用

- main.js

- ```javascript
  // 1. 创建了bus组件
  const bus = new Vue()
  // 把bus挂载到了Vue的原型上
  Vue.prototype.bus = bus
  ```

- components 中创建两个组件

- BigSon.vue

- ```vue
  <template>
    <div>
      <h3>大儿子</h3>
      配偶：<input type="text" v-model="wife">
      <p>弟媳：{{brotherWife}}</p>
      <button @click="send">传值</button>
    </div>
  </template>
  
  <script>
  export default {
    created () {
      this.bus.$on('sb', value => {
        this.brotherWife = value
      })
    },
    data () {
      return {
        wife: '',
        brotherWife: ''
      }
    },
    methods: {
      send () {
        this.bus.$emit('send', this.wife)
      }
    }
  }
  </script>
  
  <style>
      
  </style>
  ```

- SmallSon.vue

- ```vue
  <template>
    <div>
      <h3>小儿子</h3>
      <!-- <button @click="fn">订阅</button> -->
      配偶：<input type="text" v-model="wife"> <button @click="fn">公布</button>
      <div>大嫂：{{bigSao}}</div>
    </div>
  </template>
  
  <script>
  export default {
    data () {
      return {
        bigSao: '',
        wife: ''
      }
    },
    // // 订阅事件
    // methods: {
    //   fn () {
        
    //   }
    // }
    // 组件刚创建的时候会自动执行
    created () {
      // console.log('123')
      this.bus.$on('send', (value) => {
        console.log(value)
        this.bigSao = value
      })
    },
    methods: {
      fn () {
        this.bus.$emit('sb', this.wife)
      }
    }
  }
  </script>
  
  <style>
  
  </style>
  
  ```

- app.vue

- ```vue
  <template>
    <div>
      <h1>父盒子</h1>
      大儿媳妇：{{bigSonWife}}
      小儿媳妇：
      <hr>
      <big-son></big-son>
      <hr>
      <small-son></small-son>
    </div>
  </template>
  
  <script>
  import BigSon from './components/BigSon'
  import SmallSon from './components/SmallSon'
  export default {
    components: {
      BigSon,
      SmallSon
    },
    created () {
      this.bus.$on('send', value => {
        this.bigSonWife = value
      })
      this.bus.$on('sb', value => {
        this.bigSonWife = value
      })
    },
    data () {
      return {
        bigSonWife: ''
      }
    }
  }
  </script>
  
  <style>
  
  </style>
  ```

- 

# 生命周期

## 研究生命周期的意义

生命周期 => 一个事物从出生 到 消亡的全部过程 

生命周期（Life Cycle）是指一个组件从`创建`-> `运行` -> `销毁`的整个阶段，强调的是一个时间段



我们可以把`每个 vue 组件运行的过程`，也概括为生命周期：

- vue 组件的初始化，表示生命周期的开始
- vue 组件的销毁，表示生命周期的结束
- vue 组件中间运行的过程，就是组件的生命周期

## 生命周期函数(钩子函数)

**生命周期函数：是由 vue 框架提供的内置函数，会伴随着组件的生命周期，自动按次序执行。**

生命周期函数的作用：允许程序员在`特定的时间点`，执行某些特定的操作。

例如，组件创建完毕后，可以在created 生命周期函数中发起Ajax 请求，从而初始化 data 数据。

## 组件生命周期分类

vue 组件的生命周期函数，可以分为 3 大类：

- 组件`初始化阶段`的生命周期函数
- 组件`运行阶段`的生命周期函数
- 组件`销毁阶段`的生命周期函数

```js
1. beforecreated：data数据初始化之前，组件还没有数据
2. created: data数据初始化之后，可以获取到组件的数据
3. beforeMount：DOM渲染之前，DOM还没渲染
4. mounted：DOM渲染之后，可以操作DOM了
5. beforeUpdate: 数据更新，DOM更新前
6. updated: 数据更新，DOM更新后
7. beforeDestroy: 组件销毁前
8. destroyed: 组件销毁后
```

```javascript
  data () {
    return {
      msg: 'hello'
    }
  },
  methods: {
    fn () {
      // 销毁这个组件
      this.$destroy()
    }
  },
  // data methods computed watch filters components name 8个钩子  
  beforeCreate () {
    console.log('beforeCreate执行', '数据初始化之前')
    console.log(this.msg)
  },
  created () {
    this.timer = setInterval(() => {
      console.log('哈哈哈，我是App')
    }, 1000)
    console.log('created执行', '数据初始化之后')
    console.log(this.msg)
  },
  beforeMount () {
    console.log('beforeMount', 'DOM初始化之前')
    console.log(document.getElementById('app').innerHTML)
  },
  mounted () {
    console.log('mounted', 'DOM初始化之后')
    console.log(document.getElementById('app').innerHTML)
  },
  beforeUpdate () {
    console.log('beforeUpdate', '数据发生改变了， 但是DOM还没有更新')
    console.log(document.getElementById('app').innerHTML)
  },
  updated () {
    console.log('updated', '数据发生改变了， DOM更新完成')
    console.log(document.getElementById('app').innerHTML)
  },
  // 调用this.$destroy()可以手动进行销毁。
  beforeDestroy() {
    clearInterval(this.timer)
    console.log('beforeDestroy', '销毁前')
  },
  destroyed () {
    console.log('destroyed', '销毁后')
  }
```

# axios的使用

1. axios 是什么 ?

   就是一个 发送  ajax  请求的工具。类似于`$.ajax`

   axios 底层, 就是 原生 ajax, 只是它内部是通过 promise 封装

2. axios的基本使用

```jsx
axios({
  method: '请求方式', // get post
  url: '请求地址',
  data: {    // 拼接到请求体的参数,  post请求的参数
    xxx: xxx,
  },
  params: {  // 拼接到请求行的参数, get请求的参数
   	xxx: xxx 
  }
}).then(res => {
  console.log(res.data) // 后台返回的结果
}).catch(err => {
  console.log(err)
})

```

	3. axios演示

```javascript
export default {
  async created () {
    // 发送请求的配置信息
    // method:  请求方式  默认是get
    // url:     请求地址  
    // data:    请求体数据 适合post请求
    // params:  url参数    适合get请求
    // 基于promise封装，，，
    const res = await axios({
      method: 'get',
      url: 'https://www.escook.cn/api/cart'
    })
    console.log(res)
  }
}
```

# ref 和 $refs 

reference-引用、参考

利用 ref 和 $refs 可以用于获取 dom 元素, 或者组件实例

每个 vue 的组件实例上，都包含一个$refs 对象，里面存储着对应的DOM 元素或组件的引用。

1 给需要获取的 dom 元素或者组件, 添加 ref 属性

```jsx
<div>
  <div ref="box">我是div盒子</div>
  <jack ref="jack"></jack>
  <button @click="fn">按钮</button>
</div>
```

2 通过 `this.$refs.xxx` 获取, 拿到组件可以调用组件的方法

```jsx
import Jack from './jack.vue'
export default {
  methods: {
    fn () {
      console.log(this.$refs.box)
      console.log(this.$refs.jack)
      this.$refs.jack.sayHi()
    }
  },
  components: {
    Jack
  }
}
```

# $nextTick

**演示异步更新 **

```vue
<template>
  <div>
    <h1>DOM的异步更新</h1>
    <div ref="a">{{count}}</div>
    <button @click="fn">修改</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'hello',
      count: 0,
    }
  },
  methods: {
    fn () {
      for(var i = 0; i < 1000000; i++) {
        this.count++
      }
      console.log(this.$refs.a.innerText)
    }
  }
}
</script>

<style>

</style>
```



- vue只要数据发生了改变，会自动更新对应的DOM解构
- vue更新完数据，不是立即更新DOM，而是异步更新DOM

**需求1: 点击按钮, 切换显示输入框**

```vue
<template>
  <div>
    <!-- 需求: 点击按钮, 切换显示输入框 -->
    <input type="text" v-if="isShowInput">
    <button @click="fn1" v-else>点此搜索</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isShowInput: false
    }
  },
  methods: {
    fn1 () {
      this.isShowInput = true
    }
  }
}
</script>
```

**需求2: 显示输入框的同时, 要获取焦点**

当文本框展示出来之后，如果希望它立即获得焦点，则可以为其添加 ref 引用，并调用原生 DOM 对象的.focus() 方法即可。

直接调用会报错, 因为 vue 是 异步dom更新的 (提升渲染效率),  `this.isShowInput = true` 执行完时, 实际的 dom 还没渲染出来

```jsx
<input ref="inp" type="text" v-if="isShowInput">

fn () {
  this.isShowInput = true
  this.$refs.inp.focus()
}
```

组件的 `$nextTick(callback)` 方法，会把 callback 回调推迟到下一个 DOM 更新周期之后执行。

通俗的理解是：**等组件的DOM 刷新之后，再执行 callback 回调函数**。从而能保证 callback 函数可以操作到最新的 DOM 元素。

```vue
<template>
  <div>
    <!-- 需求: 点击按钮, 切换显示输入框 -->
    <input ref="inp" type="text" v-if="isShowInput">
    <button @click="fn" v-else>点此搜索</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isShowInput: false
    }
  },
  methods: {
    async fn () {
      this.isShowInput = true
      // nextTick的回调函数不是立即执行的，而是会在DOM更新后立马执行。
      // this.$nextTick(function () {
      //   this.$refs.input.focus()
      // })
      // 如果不传回调函数，nextTick会返回promise对象
      // this.$nextTick().then(() => {
      //   console.log('DOM更新好了')
      //   this.$refs.input.focus()
      // })
      // 等待DOM更新
      await this.$nextTick()
      this.$refs.input.focus()
    }
  }
}
</script>
```

# dynamic 动态组件

## 动态组件的基本使用

什么是动态组件:   让多个组件使用同一个挂载点，并动态切换，这就是动态组件 

```vue
<template>
  <div>
    <h3>动态组件的演示</h3>
    <!-- 动态组件 => 多个组件使用同一个挂载点, 并可以动态的切换展示 -->
    <button @click="comName = 'my-swiper'">swiper</button>
    <button @click="comName = 'my-nav'">nav</button>
    
    <!-- 
      <my-nav></my-nav>
      <my-swiper></my-swiper> 
    -->
    <component :is="comName"></component>
  </div>
</template>

<script>
import MyNav from './my-nav.vue'
import MySwiper from './my-swiper.vue'
export default {
  data () {
    return {
      comName: 'my-nav'
    }
  },
  components: {
    MyNav,
    MySwiper
  }
}
</script>
```

## 使用 keep-alive 保持状态

默认情况下，切换动态组件时无法保持组件的状态。会将组件销毁, 将来显示时, 又会重新创建。

创建register.vue和login.vue两个组件，动态调用的组件中增加两个钩子函数：

```javascript
  // 每次切换的时候组件都会被重新创建和销毁  
  created() {
    console.log('创建了')
  },
  beforeDestroy () {
    console.log('销毁了')
  },
```



可以使用vue 内置的 `<keep-alive>` 组件保持动态组件的状态。

使用 keep-alive 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们

```jsx
<keep-alive>
  <component :is="comName"></component>
</keep-alive>
```

==注意：缓存的组件是不会被销毁的，所以beforeDestroy和Destroyed两个钩子函数不会执行==

```jsx
缓存组件有两个新增的钩子函数
// 缓存的组件还有额外的两个钩子函数
// 缓存组件激活的时候触发
activated () {
  console.log('login组件激活了')
},
// 缓存组件失活的时候触发
deactivated () {
  console.log('login组件失活了')
}
```

Q&A：

​	1 什么场景要是用动态组件？

​	2 什么场景要要是用keep-alive标签？	 

# 插槽

插槽（Slot）是 vue 为组件的封装者提供的能力。

允许开发者在封装组件时，把不确定的、希望由用户指定的部分定义为插槽。

## 默认插槽 slot

**需求: 要在页面中显示一个对话框, 封装成一个组件**

子组件简单实现功能Modal.vue：

```vue
<template>
  <div>
    <div class="header">
      我是标题
    </div>
    <div class="content">
      我是内容
    </div>
    <div class="footer">
      <button>确定</button>
      <button>取消</button>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style>

</style>
```



通过父传子, 固然可以完成一定层面的组件的定制, 但是自定义性较差, 

如果希望能够自定义组件内部的一些结构 => 就需要用到插槽

### 默认插槽基本使用

**插槽作用: 用于实现组件的内容分发, 通过 slot 标签, 可以接收到写在组件标签内的内容**

插槽：slot  作用：占位置

基本示例:

```jsx
<my-dialog>
  <p>请输入正确的手机号码</p>
</my-dialog>
```

`my-dialog.vue`

```less
<template>
  <div class="my-dialog">
    <div class="header">
      <h3>友情提示</h3>
    </div>
    <div class="content">
      <slot></slot>
    </div>
    <div class="footer">
      <button>关闭</button>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
.my-dialog {
  width: 400px;
  padding: 10px 20px;
  border: 3px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```



### 后备内容 (默认值)

封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）。

如果组件的使用者没有为插槽提供任何内容，则后备内容会生效。

```jsx
<template>
  <div class="my-dialog">
    <div class="header">
      <h3>友情提示</h3>
    </div>
    <div class="content">
      <slot>这是后备内容</slot>
    </div>
    <div class="footer">
      <button>关闭</button>
    </div>
  </div>
</template>
```

Q&A：

​	1 什么场景要想到使用插槽slot

## 具名插槽

### 插槽的分类:

**1 默认插槽(匿名插槽)**

`<slot></slot>` 只要没有具体分发的内容, 都会给到默认插槽

`<slot name="default"></slot>` 是默认插槽完整的写法 和 `<slot></slot>` 完全等价

**2 具名插槽: 具有名字的插槽 (配置了名字),  可以实现定向分发**

一旦配置了名字, 只会接收对应的内容, 不是分发给他的, 就不要



### 具名插槽的使用步骤

(1) 给插槽起名字 

```jsx
<div class="header">
  <slot name="header"></slot>
</div>
<div class="content">
  <slot>这是后备内容</slot>
</div>
<div class="footer">
  <slot name="footer"></slot>
</div>
```

(2) 需要使用 template 标签, 将内容包裹成一个整体

(3) 通过 v-slot:插槽名, 指定具体分发给谁

```html
<my-dialog>
  <template v-slot:header>
    <h3>这是大标题</h3>
  </template>

  <template v-slot:default>
    <p>这是内容</p>
  </template>

  <template v-slot:footer>
    <button>确认</button>
    <button>取消</button>
  </template>
</my-dialog>
```



### 具名插槽的简写

跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #。

例如 v-slot:header 可以被简写为 #header

```jsx
<my-dialog>
  <template #header>
    <h3>这是大标题</h3>
  </template>

  <template #default>
    <p>这是内容</p>
  </template>

  <template #footer>
    <button>确认</button>
    <button>取消</button>
  </template>
</my-dialog>
```

## 作用域插槽

作用域插槽: **定义 slot 插槽的同时, 是可以传值的**, 将来在分发内容时, 可以使用

1. 给 slot 标签, 以 添加属性的方式传值

```jsx
<slot name="bottom" :yes="yes" :no="no" money="100"></slot>
```

2. 所有添加的属性, 都会被收集到一个对象中

```js
{ yes: '确认', no: '取消', money: '100' }
```

3. 在template中, 通过  `v-slot:插槽名= "obj"` 接收

```jsx
<template #bottom="obj">
  <!-- {{ obj }} -->
  <button>{{ obj.yes }}</button>
  <button>{{ obj.no }}</button>
  <button>{{ obj.money }}</button>
</template>
```

4. 可以使用解构赋值简化数据的接收

```jsx
<template #bottom="{ yes, no, money }">
  <button>{{ yes }}</button>
  <button>{{ no }}</button>
  <button>{{ money }}</button>
</template>
```

# 自定义指令

## 自定义指令说明

实现操作DOM示例：

```vue
// 通过标签的属性可以让标签自动获取焦点，但是兼容性不好
<input ref="inp" type="text" autofocus>

// 也可以在mounted钩子函数中操作DOM，让标签自动获得焦点。
<input ref="inp" type="text">

// DOM渲染完成的钩子函数
mounted () {
  this.$refs.input.focus()
}

```



除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，Vue 也允许注册自定义指令。 `v-xxx`  

注意，代码复用和抽象的主要形式是组件。

然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。

## 自定义指令 - 局部注册

例如需求:  当页面加载时，让元素将获得焦点 , (autofocus 在 safari 浏览器有兼容性)

```less
<template>
  <div>
    <h3>自定义指令</h3>
    <input type="text" v-focus>
  </div>
</template>

<script>
export default {
  directives: {
    // 自定义一个局部指令
    focus: {
      inserted (el) {
        el.focus()
      }
    }
  }
}
</script>
```

## 自定义指令 - 全局注册

```jsx
// 注册全局自定义指令
Vue.directive('focus', {
  inserted (el) {
    el.focus()
  }
})
```

## 自定义指令 - 指令的值

在绑定指令时，可以通过“等号”的形式为指令绑定具体的参数值

需求: v-color="color" 给对应的颜色, 就能改对应的字体颜色

```jsx
<div v-color="color">我是内容</div>
```

实现:

```jsx
directives: {
  // 自定义一个局部指令
  color: {
    // 指令所在的元素渲染的时候
    inserted (el, {value}) {
      el.style.color = value
    },
    // update指令的值改变时触发, binding.value指令的值修改触发
    update (el, binding) {
      el.style.color = binding.value
    }
  }
}
```

## v-model语法糖

> 语法糖：v-model本质上是 value属性和input事件的一层包装

```jsx
/* 
  v-model的作用：提供数据的双向绑定
    数据发生了改变，页面会自动变  v-bind:value
    页面输入的时候（改变） 数据会自动变化  v-on:input
  	v-model是语法糖， v-model等价于 给一个input框提供了 :value属性以及 @input事件，但是如果每次使用input框，都需要提供value和input事件比较麻烦，所以使用v-model
*/

<template>
  <div>
    <input type="text" v-model="msg">
    <input type="text" :value="msg" @input="msg = $event.target.value">

    <input type="text" :value="car" @input="car = $event.target.value">
    <input type="text" v-model="car">
  </div>
</template>
```



## v-model给表单元素使用

> 我们经常遇到一种场景： 父组件提供一个数据给子组件使用（父传子），子组件需要修改父组件的数据，所以需要子传父把值传给父组件。 这种场景可以使用v-model进行简写。

+ 定义组件的时候，注意接收的值叫value,子传父触发的事件叫 input

## tabbar组件使用v-model优化：

app.vue

```vue
<my-tab-bar :tabList="tabList" v-model="active"></my-tab-bar>

```

MyTabBar.vue

```vue
	// props接收的参数名改成value
	value: {
      type: Number,
      default: 0
    }
```

```vue
	// 触发事件的事件名改成 input
	changeTab (index) {
      this.$emit('input', index)
    }
```



# 单页应用程序与路由

## SPA - 单页应用程序

- SPA： `Single Page Application`  单页面应用程序
- MPA : `Multiple Page Application`多页面应用程序 

### 优势

- 传统的多页面应用程序，每次请求服务器返回的都是一整个完整的页面
- 单页面应用程序只有第一次会加载完整的页面
- 以后每次请求仅仅获取必要的数据，减少了请求体积，加快页面响应速度，降低了对服务器的压力
- SPA更好的用户体验，运行更加流畅

### 缺点

1. 开发成本高 (需要学习路由)  `vue-router   react-router`
2. **不利于 SEO** 搜索引擎优化    谷歌浏览器在解决这个问题    ssr:服务端渲染 server side rendering

## 路由介绍

- **路由** : 是浏览器 **URL 中的哈希值**( # hash) 与 **展示视图内容(组件)** 之间的对应规则
  - 简单来说,路由就是一套映射规则(一对一的对应规则), 由开发人员制定规则.- 
  - 当 URL 中的哈希值( `#` hash) 发生改变后,路由会根据制定好的**规则**, 展示对应的视图内容(组件)
- **为什么要学习路由?**
  - 渐进式 =>vue => vuer-router (管理组件之间的跳转)
  - 在 web App 中, 经常会出现通过一个页面来展示和管理整个应用的功能.
  - SPA 往往是功能复杂的应用,为了有效管理所有视图内容,前端路由 应运而生.
- **vue 中的路由** : 是 **hash** 和 **component** 的对应关系, **一个哈希值对应一个组件**

## 前端路由 - 工作模式原理 (手写)

基本思路:

1. 用户点击了页面上的路由链接
2. 导致了 URL 地址栏中的 Hash 值发生了变化
3. 前端路由监听了到 Hash 地址的变化
4. 前端路由把当前 Hash 地址对应的组件渲染都浏览器中

实现简单的前端路由:

1. 导入并注册 `my-home.vue`  `my-movie`  ` my-about` 三个组件

```html
<script>
import MyAbout from './components/my-about.vue'
import MyHome from './components/my-home.vue'
import MyMovie from './components/my-movie.vue'
export default {
  components: {
    MyHome,
    MyAbout,
    MyMovie
  }
}
</script>
```

2. 通过 comName 动态组件, 控制要显示的组件

```vue
<template>
  <div>
    <h1>App组件</h1>
    <component :is="comName"></component>
  </div>
</template>

<script>
import MyAbout from './components/my-about.vue'
import MyHome from './components/my-home.vue'
import MyMovie from './components/my-movie.vue'
export default {
  data () {
    return {
      comName: 'my-home'
    }
  },
  components: {
    MyHome,
    MyAbout,
    MyMovie
  }
}
</script>
```

3. 声明三个导航链接, 点击时修改地址栏的 hash 值

```jsx
<template>
  <div>
    <h1>App组件</h1>
    <a href="#/home">首页</a>&nbsp;
    <a href="#/movie">电影</a>&nbsp;
    <a href="#/about">关于</a>&nbsp;
    <component :is="comName"></component>
  </div>
</template>
```

4. 在 created 中, 监视地址栏 hash 时的变化, 一旦变化, 动态切换展示的组件

```jsx
created () {
  window.onhashchange = () => {
    console.log(location.hash)
    switch(location.hash) {
      case '#/home':
        this.comName = 'my-home'
        break
      case '#/movie':
        this.comName = 'my-movie'
        break
      case '#/about':
        this.comName = 'my-about'
        break
    }
  }
},
```

# vue-router

用于开发SPA，vue-router的核心功能就是处理前端路由

## 基本使用

- 安装

```bash
yarn add vue-router
```

+ 导入路由

```js
import VueRouter from 'vue-router'
```

+ 使用路由插件

```jsx
// 在vue中，使用使用vue的插件，都需要调用Vue.use()
Vue.use(VueRouter)
```

+ 创建路由对象

```jsx
const router = new VueRouter()
```

+ 关联到vue实例

```jsx
new Vue({
  router
})
```

+ 查看页面是否生效



## 配置路由规则

+ 在new VueRouter()通过 routes参数配置路由的规则

```jsx
import Home from './views/Home'
import Music from './views/Music'
import Friend from './views/Friend'

const router = new VueRouter({
  // route 一条路由规则
  // routes 多条路由规则
  routes: [
    // 一条规则 path: 路由，锚点值  component 组件
    { path: '/home', component: Home },
    { path: '/music', component: Music },
    { path: '/friend', component: Friend },
  ]
})
```

+ 配置了规则，一定要指定路由的出口 

```jsx
<!-- 路由的出口，，可以理解为 component -->
<router-view></router-view>
```

router-view 会渲染成路由规则匹配的组件

## 导航链接

可以通过a链接或者通过vue-router提供的 router-link标签指定导航链接

```jsx
<!-- vue-router提供了一个组件 router-link: 作用用于提供路由链接 -->
<!-- router-link实质上最终会渲染成a链接 to属性等价于提供 href属性-->
<!-- router-link提供了导航链接高亮的功能 -->
<ul>
  <li><a href="#/home">首页</a></li>
  <li><a href="#/music">音乐</a></li>
  <li><a href="#/friend">朋友</a></li>
</ul>

<ul>
  <li><router-link to="/home">首页</router-link></li>
  <li><router-link to="/music">音乐</router-link></li>
  <li><router-link to="/friend">朋友</router-link></li>
</ul>
```

推荐使用router-link实现导航链接，因为router-link会自动增加两个类名 router-link-active   router-link-exact-active，通过这两个类名就可以实现当前当行高亮。



## 具体步骤

实现vue的具体步骤

+ 配置路由规则  hash值和组件的映射规则
+ 提供对应组件
+ 配置路由的显示出口, 确定匹配到的组件显示的位置

配置路由规则

```js
const router = new VueRouter({
    // 配置路由的规则
    routes: [
        { path: '/one', component: One }, 
        { path: '/two', component: Two }
    ]
})
```

创建对应组件 views/One.vue  views/Two.vue

配置路由的出口，显示位置

```html
<div id="app">
  <router-view></router-view>
</div>
```

## 路由导航

```html
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
```

Q&A：

​	1使用router-link的好处是什么？

## 导航高亮

- **点击导航 =**> 元素里添加了两个类

```html
<a href="#/one" class="router-link-exact-active router-link-active">One</a>
<a href="#/two" class="">Two</a>
```

- **修改方式1 : 直接修改类的样式**

```css
.router-link-exact-active,
.router-link-active {
  color: red;
  font-size: 50px;
}
```

精确匹配和模糊匹配

- 精确匹配 : router-link-exact-active 类名 : 只有当 `浏览器地址栏中的哈希值 与 router-link 的 to 属性值,完全匹配对,才会添加该类`
- 模糊匹配: router-link-active 类名 : 只要 `浏览器地址栏中的哈希值` 包含 router-link 的 to 属性值,就会添加该类名
- 解决办法 : 加个 exact

```js
<router-link to="/" exact>
  One
</router-link>
```

## 嵌套路由

+ 给子路由创建组件 Login.vue和Register.vue
+ 给某个路由的children属性配置子路由

+ 还需要在组件中配置子路由的出口

## 编程式导航

```
需求：登录功能，登录成功了，需要从 /login 跳转到 /home
// http://hucongcong.gitee.io/hmpc
1. 声明式导航 通过router-link就可以   router-link叫做  
2. 编程式导航  通过js代码实现路由的跳转，就叫编程式导航 router.push()
```

通过js的方式实现路由的跳转就叫编程式导航

**在 Vue 实例内部，你可以通过 $router 访问路由实例。因此你可以调用 this.$router.push**

## 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

## 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

每个守卫方法接收三个参数：

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)

- **`from: Route`**: 当前导航正要离开的路由

- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。

  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。

  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。

  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

    **确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

    ```js
    // BAD
    router.beforeEach((to, from, next) => {
      if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
      // 如果用户未能验证身份，则 `next` 会被调用两次
      next()
    })
    // GOOD
    router.beforeEach((to, from, next) => {
      if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
      else next()
    })
    ```