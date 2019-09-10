---
title: 'webpack基本使用配置[基础篇]'
tags:
  - webpack
abbrlink: 33486
date: 2016-10-08 20:28:47
---

<div class="foreword">webpack是一个前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。还可以结合gulp使用，不冲突。
</div>

<!-- more -->

### webpack优势
- 模块来源广泛，支持包括npm/bower等等的各种主流模块安装／依赖解决方案；
- 模块化规范支持全面，AMD/CommonJS一应具全；
- 插件机制完善，实现本身实现同样模块化，容易扩展；
- Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。

### 安装
首先要安装 [Node.js](http://nodejs.org)， Node.js 自带了软件包管理器 npm，Webpack 需要 Node.js v0.6 以上支持，建议使用最新版 Node.js。
用 npm 全局安装 Webpack：
``` javascript
$ npm install webpack -g
```
初始化配置文件 package.json ：
```
$ npm init
```
到项目目录安装，将 webpack 添加到 package.json ：
```
$ npm install webpack --save-dev
```
### webpack常用命令
- webpack 最基本的启动webpack命令
- webpack -w 提供watch方法，实时进行打包更新
- webpack -p 对打包后的文件进行压缩
- webpack -d 提供SourceMaps，方便调试
- webpack --colors 输出结果带彩色，比如：会用红色显示耗时较长的步骤
- webpack --profile 输出性能数据，可以看到每一步的耗时
- webpack --display-modules 默认情况下 node_modules 下的模块会被隐藏，加上这个参数可以显示这些被隐藏的模块

前面的四个命令比较基础，使用频率会比较大，后面的命令主要是用来定位打包时间较长的原因，方便改进配置文件，提高打包效率。

### 例子
首先创建一个静态页面 index.html 和一个 JS 入口文件 entry.js：
```
<!-- index.html -->
<html>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```
```
// entry.js
document.write('It works.')
```
然后编译 entry.js 并打包到 bundle.js：
```
$ webpack entry.js bundle.js
```
打包过程会显示日志：
```
Hash: e964f90ec65eb2c29bb9
Version: webpack 1.12.2
Time: 54ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.42 kB       0  [emitted]  main
   [0] ./entry.js 27 bytes {0} [built]
```
用浏览器打开 index.html 将会看到 It works.
