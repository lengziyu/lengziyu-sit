---
title: fetch 学习笔记
tags: fetch
categories: fetch
abbrlink: 53312
date: 2016-10-23 19:58:14
---
XMLHttpRequest 是一个设计粗糙的 API，不符合关注分离（Separation of Concerns）的原则，配置和调用方式非常混乱，而且基于事件的异步模型写起来也没有现代的 Promise，generator/yield，async/await 友好。
<!--more-->
## 安装
npm ：
```
$ npm install whatwg-fetch --save
```
如果你项目在node.js环境运行，可以使用 [node-fetch](https://github.com/bitinn/node-fetch).

对于 babel 和 es2015+，可以这样导入 fetch：
```
import 'whatwg-fetch';
fetch(...);
```
## 兼容性及解决方案
原生支持率并不高，幸运的是，引入下面这些 `polyfill` 后可以完美支持 IE8+ ：

- 由于 IE8 是 ES3，需要引入 ES5 的 `polyfill`: `es5-shim`, `es5-sham`；
- 引入 Promise 的 `polyfill`: `es6-promise`；
- 引入 fetch 探测库：fetch-detector；
- 引入 fetch 的 polyfill: fetch-ie8；
- 可选：如果你还使用了 jsonp，引入 fetch-jsonp；

## 使用
fetch 支持 HTTP 方法，下面主要用例子讲解 POST 和 GET 的请求。
**HTML 请求：**
```
fetch('/users.html')
  .then(function(response) {
    return response.text()
  }).then(function(body) {
    document.body.innerHTML = body
  })
```
**JSON 请求：**
```
fetch('/users.json')
  .then(function(response) {
    return response.json()
  }).then(function(json) {
    console.log('parsed json', json)
  }).catch(function(ex) {
    console.log('parsing failed', ex)
  })
```
**响应头设置：**
```
fetch('/users.json').then(function(response) {
  console.log(response.headers.get('Content-Type'))
  console.log(response.headers.get('Date'))
  console.log(response.status)
  console.log(response.statusText)
})
```
**Post 表单提交：**
```
var form = document.querySelector('form')

fetch('/users', {
  method: 'POST',
  body: new FormData(form)
})
```
**Post JSON：**
```
fetch('/users', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Hubot',
    login: 'hubot',
  })
})
```
**上传文件：**
```
var input = document.querySelector('input[type="file"]')

var data = new FormData()
data.append('file', input.files[0])
data.append('user', 'hubot')

fetch('/avatars', {
  method: 'POST',
  body: data
})
```
## 使用注意：
- Fetch 请求默认是不带 cookie 的，需要设置 `fetch(url, {credentials: 'include'})`
- 服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
