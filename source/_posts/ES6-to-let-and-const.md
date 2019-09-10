---
title: ES6笔记之 let 和 const
tags: es6
categories: es6
abbrlink: 19172
date: 2016-10-17 21:51:50
---
ES6新增了 let 和 const 命令，用来声明变量。它的用法类似于var，却有所区别。
<!--more-->
## let
let声明的变量只在其所在的代码块内有效。

```
{
    let a = 1;
}
console.log(a)   //ReferenceError:a is not defined
```


**let声明变量不存在变量提升**
let不像var那样会发生“变量提升”现象，所以，变量一定要在声明后使用，不然就会报错。

```
console.log(a)   //ReferenceError:a is not defined
let a = 1;
```


**暂时性死区**
只要块级作用域内存在let关键字，它所声明的变量就绑定这个区域，不再受外部影响。

```
var tmp = 20;
if(true){
    tmp = 'abc';    //ReferenceError:tmp is not defined
    let tmp;
}
```
上面的代码中存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定了这个块级作用域，所以在let声明变量前，对tmp赋值会报错。
<div class="tip">ES6明确规定，如果区块中存在let和const命令，则这个区块对这些命令声明的变量从一开始就形成封闭作用域。只要在声明之前使用这些变量，就会报错。在语法上称为“暂时性死区(temporal dead zone,TDZ)”</div>有时候，会不经间遇到比较隐蔽的“死区”，不太容易被发现。
```
function bar(x=y,y=2){
    return [x,y];
}
bar() //报错
```
上面的代码中是因为参数x的默认值等于另一个参数y，而此时y还没有声明，属于死区。

**不允许重复声明**
let不允许在相同的作用域内声明同一个变量。
```
function bar(){
    let a = 10;
    var a = 20;
}
//报错

function bat(){
    let a = 10;
    let a = 20;
}
//报错
```
因此，所以也不能在函数内重复声明参数：
```
function bar(args){
    let args = 10;  
}
bar() //报错

function bar(args){
    {
        let args = 20;
    }
}
bar()  //不报错
```

**块级作用域**

使用let和const可以实现块级作用域：

1. 外层代码块不受内层代码块的影响。
2. 外层作用域无法读取内层作用域的变量。
3. 内层作用域可以定义外层作用域的同名变量。

块级作用域的实现，使得广泛使用的自执行匿名函数(IIFE)变得不再必要了。
```
//自执行模式
(function(){
    var a = 10;
})()


//块级作用域写法
function(){
    let a = 10;
}
```
<div class="tip">函数本身的作用域也在其所在的块级作用域之内。</div>

## const

const用来声明常量。一旦声明，其值就不能再改变。
```
const PI = 3.1415;
const PI = 3  //TypeErrorL "PI" is read-only
```
<div class="tip">const声明的变量不得改变值，意味着 const 一旦声明常量就必须立即初始化，不能留到后面赋值。</div>
const与let关键字一样，只在声明所在的块级作用域内有效；const关键字声明的常量也不提升，同样存在暂时性死区，只能在声明后使用。
<div class="tip">对于复合型数据类型，常量名不指向数据，而是指向数据所在的地址。const关键字只是保证常量名指向的地址不变，并不保证该地址的数据不变，所以将一个对象声明为常量必须注意该点。</div>
