---
title: 学习TypeScript第一课
abbrlink: 34409
date: 2020-09-08 21:04:54
tags:
	- TypeScript
	- ES6
---

> TypeScript是JavaScript的一个超集，支持ECMAScript6标准，设计目标是开发大型应用。

## 安装
可以通过开发工具安装插件或用npm方式安装，这里介绍npm的方式。
```
npm i -g typescript
```
**构建一个TypeScript文件**
新建一个`greeter.ts`文件，写入以下内容。
```
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```
**编译代码**
在命令行执行如下，运行TypeScript编译：
```
tsc greeter.ts
```
输出结果为一个greeter.js文件，它包含了和输入文件中相同的JavsScript代码。

## 基础类型
|数据类型|   关键字   |   描述	|   例子
|:-|-|-|-|
|   任意类型   |   any	  |   声明为 any 的变量可以赋予任意类型的值。			|   `let x: any = 1; x = false; `
|   数字类型   |   number  |   双精度 64 位浮点值。它可以用来表示整数和分数。	|   `let decLiteral: number = 6;`
|  字符串类型  |   string   |  一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。 | `let name: string = "Runoob";`
|   布尔类型   |   boolean |   表示逻辑值`true`和`false`。   |   `let flag: boolean = true;`
|   数组类型	  |   无      |   声明变量为数组。 |  `let arr: number[] = [1, 2];` 
|    元组     |    无      |   元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。 |  `let x: [string, number]; x = ['Runoob', 1];`
|    枚举     |    enum   |   枚举类型用于定义数值集合。   					|   `enum Color {Red, Green, Blue};let c: Color = Color.Blue;`
|    void     |   void    |   用于标识方法返回值的类型，表示该方法没有返回值。   |   `function hello(): void { alert("Hello Runoob") }`
|    null     |   null    |   表示对象值缺失。                              |  无
|    undefined|   undefined|  用于初始化变量为一个未定义的值。                |   无
|   never     |   never    |  never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。 | 无

## 函数

### 函数返回值
```
function function_name():return_type { 
    // 语句
    return value; 
}

// return_type 是返回值的类型。
```
实例：
```
// 函数定义
function greet():string { // 返回一个字符串
    return "Hello World" 
} 
```
### 带参数的函数
```
function func_name( param1 [:datatype], param2 [:datatype]) {  
	
}

// param1、param2 为参数名。
// datatype 为参数类型。
```
实例：
```
function add(x: number, y: number): number {
    return x + y;
}
console.log(add(1,2))
```

### 可选参数和默认参数
**可选参数**
在 TypeScript 函数里，如果我们定义了参数，则我们必须传入这些参数，除非将这些参数设置为可选，可选参数使用问号标识 ？。
```
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
```
**参数默认值**

```
function function_name(param1[:type],param2[:type] = default_value) { 
	
}
```
注意：参数不能同时设置为可选和默认。

实例：
```
function calculate_discount(price:number, rate:number = 0.50) { 
    var discount = price * rate; 
    console.log("计算结果: ", discount); 
} 
calculate_discount(1000) 
calculate_discount(1000, 0.30)
```
