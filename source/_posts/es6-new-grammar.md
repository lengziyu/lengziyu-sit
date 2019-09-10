---
title: ES6 function* yield yield*
tags: es6
categories: es6
abbrlink: 63793
date: 2016-12-08 22:03:09
---
这里主要讲解es6新语法`function*`、`yield`和`yield*`。
<!--more-->
# function*
### 简介
定义一个generator（生成器）函数，返回一个Generator对象。

### 语法
```
function* name[ param[ param[ param…]]] { statements }
```
- name 函数名
- param 传入函数的参数名，一个函数最多可有255个参数
- statements 函数的主体

### 描述
生成器是一种可以在从中退出并在之后重新进入的函数。生成器环境在执行后会被保存，下次执行可继续使用。

调用生成器函数，并不立即执行主体，而是返回这个生成器函数的迭代器对象，当这个迭代器调用next()方法时，生成器函数主体会被执行至第一个 yield 表达式，该表达式定义了迭代器返回的值，或者，被 yield* 委托至另一个生成器函数。next() 方法返回一个对象，该对象有一个value属性，表示产出的值，和一个done属性，表示生成器是否已经产出了它最后的值。

### 基本事例
```
function* idMaker(){
  var index = 0;
  while(index<3)
    yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // undefined
```

### yield* 事例
```
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i){
  yield i;
  yield* anotherGenerator(i);
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```
### 兼容性
属于ES6规范，请使用babel转换


# yield
### 简介
yield关键词用来停止或继续一个生成器函数。

### 语法
```
yield[[ expression ]]
```
- expression 用作返回值，如果忽略，则返回undefined

### 描述
yield 关键字使生成器函数暂停执行，并返回跟在它后面的表达式的当前值. 可以把它想成是 return 关键字的一个基于生成器的版本.
yield 关键字实际返回一个对象，包含两个属性, value 和 done.  value 属性为 yield expression 的值,  done 是一个布尔值用来指示生成器函数是否已经全部完成.
一旦在 yield expression 处暂停,  除非外部调用生成器的 next() 方法，否则生成器的代码将不能继续执行. 这使得可以对生成器的执行以及渐进式的返回值进行直接控制.

### 事例
```
function* foo(){
  var index = 0;
  while (index <= 2) // when index reaches 3,
                     // yield's done will be true
                     // and its value will be undefined;
    yield index++;
}
一旦生成器函数已定义，可以通过构造一个迭代器来使用它.
var iterator = foo();
console.log(iterator.next()); // { value:0, done:false }
console.log(iterator.next()); // { value:1, done:false }
console.log(iterator.next()); // { value:2, done:false }
console.log(iterator.next()); // { value:undefined, done:true }
```

# yield*
### 简介
在生成器中，yield* 可以把需要 yield 的值委托给另外一个生成器或者其他任意的可迭代对象。

### 语法
yield*[[ expression ]]
- expression 任意可迭代的对象

### 描述
yield* 一个可迭代对象，就相当于把这个可迭代对象的所有迭代值分次 yield 出去。
yield* 表达式本身的值就是当前可迭代对象迭代完毕（当done为true时）时的返回值。


### 事例
**委托给其他生成器**
以下代码中，g1() yield 出去的每个值都会在 g2() 的 next() 方法中返回，就像那些 yield 语句是写在 g2() 里一样。
```
function* g1() {
  yield 2;
  yield 3;
  yield 4;
}

function* g2() {
  yield 1;
  yield* g1();
  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 4, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```
**委托给其他类型的可迭代对象**
除了生成器对象这一种可迭代对象，yield* 还可以 yield 其它任意的可迭代对象，比如说数组、字符串、arguments 对象等等。
```
function* g3() {
  yield* [1, 2];
  yield* "34";
  yield* arguments;
}

var iterator = g3(5, 6);

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: "3", done: false }
console.log(iterator.next()); // { value: "4", done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: 6, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```
### yield* 表达式的值
yield* 是一个表达式，不是语句，所以它会有自己的值。
```
function* g4() {
  yield* [1, 2, 3];
  return "foo";
}

var result;

function* g5() {
  result = yield* g4();
}

var iterator = g5();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true },
                              // 此时 g4() 返回了 { value: "foo", done: true }

console.log(result);          // "foo"
```
