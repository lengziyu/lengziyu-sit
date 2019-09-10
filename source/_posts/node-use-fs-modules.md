---
title: nodejs fs模块的使用
tags: node
categories: node
abbrlink: 50444
date: 2016-11-17 21:26:19
---
fs是nodejs的文件系统，使用方法均有异步和同步版本，建议大家是用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。使用fs模块可以轻松地对文件/文件夹进行操作。
<!--more-->
## 异步和同步
创建 test.txt 文件，内容如下：
```
blog.lengziyu.com
文件读取test
```
创建 file.js 文件, 代码如下：
```
var fs = require("fs");

// 异步读取
fs.readFile('input.txt', function (err, data) {
   if (err) {
       return console.error(err);
   }
   console.log("异步读取: " + data.toString());
});

// 同步读取
var data = fs.readFileSync('input.txt');
console.log("同步读取: " + data.toString());

console.log("程序执行完毕。");
```
## 打开文件
**语法**
```
fs.open(path, flags[, mode], callback)
```

**参数**
- path - 文件的路径。
- flags - 文件打开的行为。具体值详见下文。
- mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。
- callback - 回调函数，带有两个参数如：callback(err, fd)。

**例子**
接下来我们创建 file.js 文件，并打开 test.txt 文件进行读写，代码如下所示：
```
var fs = require("fs");

// 异步打开文件
console.log("准备打开文件！");
fs.open('test.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
  console.log("文件打开成功！");     
});
```
## 获取文件信息
**语法**
```
fs.stat(path, callback)
```
**参数**
- path - 文件路径。
- callback - 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象。

**例子**
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("准备打开文件！");
fs.stat('test.txt', function (err, stats) {
   if (err) {
       return console.error(err);
   }
   console.log(stats);
   console.log("读取文件信息成功！");

   // 检测文件类型
   console.log("是否为文件(isFile) ? " + stats.isFile());
   console.log("是否为目录(isDirectory) ? " + stats.isDirectory());    
});
```
## 写入文件
**语法**
```
fs.writeFile(filename, data[, options], callback)
```
**参数**
- path - 文件路径。
- data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(流) 对象。
- options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
- callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

**例子**
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("准备写入文件");
fs.writeFile('test.txt', '我是通过写入的文件内容！',  function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('test.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});
```
## 读取文件
**语法**
```
fs.read(fd, buffer, offset, length, position, callback)
```
**参数**
- fd - 通过 fs.open() 方法返回的文件描述符。
- buffer - 数据写入的缓冲区。
- offset - 缓冲区写入的写入偏移量。
- length - 要从文件中读取的字节数。
- position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
- callback - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

**例子**
test.txt 文件内容为：
```
www.lengziyu.com
```
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");
var buf = new Buffer(1024);

console.log("准备打开已存在的文件！");
fs.open('test.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }
      console.log(bytes + "  字节被读取");

      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }
   });
});
```
## 删除文件
**语法**
```
fs.unlink(path, callback)
```
**参数**
- path - 文件路径。
- callback - 回调函数，没有参数。

**例子**
test.txt 文件内容为：
```
www.lengziyu.com
```
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('test.txt', function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("文件删除成功！");
});
```
再去查看 test.txt 文件，发现已经不存在了。

## 创建目录
**语法**
```
fs.mkdir(path[, mode], callback)
```
**参数**
- path - 文件路径。
- mode - 设置目录权限，默认为 0777。
- callback - 回调函数，没有参数。

**例子**
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("创建目录 /test/");
fs.mkdir("/test/",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("目录创建成功。");
});
```

## 读取目录
**语法**
```
fs.readdir(path, callback)
```
**参数**
- path - 文件路径。
- callback - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

**例子**
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("查看 /test 目录");
fs.readdir("/test/",function(err, files){
   if (err) {
       return console.error(err);
   }
   files.forEach( function (file){
       console.log( file );
   });
});
```
## 删除目录
**语法**
```
fs.rmdir(path, callback)
```
**参数**
- path - 文件路径。
- callback - 回调函数，没有参数。

**例子**
接下来我们创建 file.js 文件，代码如下所示：
```
var fs = require("fs");

console.log("准备删除目录 /tmp/test");
fs.rmdir("/tmp/test",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("读取 /tmp 目录");
   fs.readdir("/tmp/",function(err, files){
      if (err) {
          return console.error(err);
      }
      files.forEach( function (file){
          console.log( file );
      });
   });
});
```
更多内容，请查看官网文件模块描述：[File System](https://nodejs.org/api/fs.html#fs_fs_rename_oldpath_newpath_callback)。
