## 命令行

- node -v
- npm -v
- npm install npm@latest -g
- npm install npm@next -g
- where node
- package.json Commonjs 规范中包规范的一部分;

  - 而 nodejs 是基于 commonjs 规范的，所以只要一谈到 node、node 包、node 第三方包，都会有 package.json 的存在；
  - 而 npm 是管理包的最好方式，所以 npm 也会看到 package.json，并且 Npm 是初始化 package.json 的方式之一。

## 参考

- 关于 Commonjs\package.json 可以参考

  > <https://javascript.ruanyifeng.com/nodejs/module.html>
  >
  > <https://www.jianshu.com/p/57442f168563>
  >
  > <https://segmentfault.com/a/1190000015337690>
  >
  > https://juejin.im/post/5de5e003e51d4504b85cd15b
  >
  > https://cloud.tencent.com/developer/article/1014798

---

- webpack npm node 之间关系？

  > webpack 是 npm 生态中的一个模块，我们可以通过全局安装 webpack 来使用 webpack 对项目进行打包；
  > webpack 的运行依赖于 node 的环境，没有 node 是不能打包的，但是 webpack 打包后的项目本身只是前端静态资源和后台没有关系，也就是说不依赖与 node，只要有后台能力的都可以部署项目
  > npm 是于 Node 社区中产生的，是 nodejs 的官方包管理工具，当你下载安装好 node 的时候，npm cli 就自动安装好了
  > 正是因为 npm 的包管理，使得项目可以模块化的开发，而模块化的开发带来的这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别，而手动处理又是非常繁琐的，这就是 webpack 工具存在的意义

# nodejs

- [参考](https://www.ixigua.com/i6701880893633462791/)

- nodejs 的单线程指的是 nodejs 的代码运行的单线程的环境中，而不是指的 js 代码是单线程的。运行在 libuv。

## nodejs 模块关注的三个方面

- 模块的方法

- 模块的属性

- 模块的事件

## nodejs 三部分组成

- EMPSscript 的核心实现

- 全局变量

- 自己的模块

## nodejs 相关网站

- [官网](https://nodejs.org)
- [中文文档](http://nodejs.cn)
- [libuv](http://libuv.org)
- [包管理](https://npmjs.org)
- [WEBServer 框架](http://expressjs.com/)
- [WEBServer 框架](https://koajs.com/)
- [Socket 通信框架](https://socket.io/)
- [桌面应用框架](https://electronjs.org/)
- [第三方包与资源](https://github.com/sindresorhus/awesome-nodejs)

## nodejs 调试的一些命令

- nvm
- curl
- vim
- node inspect \*.js 配合 chrome://inspect

## nodejs 模块相关

- nodejs 不像其他编程语言，有入口函数，比如 main；nodejs 没有入口函数，所以 node 一开始就想到了模块，也避免了变更的污染；

- 一个 js 文件就是一个模块，哪怕这个模块没有任何代码，那也是一个模块；

- 一般一个第三方工具包会有多个模块，require 时，是查找的这个工具包的 index.js 文件；如果没有，那个第个模块的 package.json 中会写明入口文件的文件名；

- require 查找文件顺序请参考[require() 源码解读](http://www.ruanyifeng.com/blog/2015/05/require.html)

- > ```javascript
  > (function (exports, require, module, __filename, __dirname) {
  >   // 模块源码
  > });
  > ```

  也就是说，模块的加载实质上就是，注入 exports、require、module 三个全局变量，然后**执行模块的源码**，然后将模块的 exports 变量的值输出。

- index.js 文件作为入口文件，是 nodejs 官方默认规定的；

- 一个模块对外暴露的属性，方法，一般通过

  ```
   module.exprots={
       name:name,
       getName:getName
   }

  ```

## nodejs 同步与异步

- callback 回调函数

- promise:bluebird\promisify

- generator/yield(ES6):co

- async/wait(ES7)

  > 说明：callback 会有回调地狱的情况，如多个回调的时候
  >
  > ```
  > callback(err,stat)={
  > 	......
  > 	callback(err,stat)=>{
  > 		......
  > 		callback(err,stat)=>{
  > 			......
  > 		}
  > 	}
  > }
  > ```
  >
  > 这种情况出现，发现代码非常难读，不易维护；
  >
  > 为了解决这种情况出现了 promise 和 generator
  >
  > 等 ES7 出来时就会有 async/wait 这是 一个终极解决方案，也是基于 promise 实现的；

## nodejs 自定义全局变量的两种方式

- global.xxx=123
- xxx=123
- 以上两种变量定义方式没有加 let 、const、var

## nodejs 中的 exports 与 module.exports

- exports = module.exports = {}

- ```
  //utils.js
  let a = 100;

  console.log(module.exports); //能打印出结果为：{}
  console.log(exports); //能打印出结果为：{}

  exports.a = 200; //这里辛苦劳作帮 module.exports 的内容给改成 {a : 200}

  exports = '指向其他内存区'; //这里把exports的指向指走

  //test.js

  var a = require('/utils');
  console.log(a) // 打印为 {a : 200}
  ```
