---
title: React 操作真实 DOM
tags: react
categories: react
abbrlink: 31953
date: 2016-10-20 20:18:07
---
React中的每一个组件都是一个状态机，通常情况下，我们通过设置组件的状态就可以完成UI的更新，但是在某些情况下确实需要直接操作DOM。
<!--more-->
React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render() 输出的任何组件上。

- ref : 绑定属性
- refs : 调用的时候使用

## 获取DOM实例
通过ref属性，你可获取实例中的属性方法，甚至可以通过他获取到DOM实例节点 `this.refs.xx.getDOMNode()`

**ref 属性绑定**
```
<input type="text" ref="myInput" />
```
**refs 获取DOM实例**
获取支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。
```
// 输入框获取焦点
this.refs.myInput.focus()
```
**示例**
```
import React, { Component } from 'react';
class MyComponent extends Component {
  handleClick(){
    this.refs.myInput.focus();
  }
  render(){
    return(
      <div>
        <input
          type="text"
          ref="myInput"
        />
        <input
          type="button"
          value="点我输入框获取焦点"
          onClick={this.handleClick.bind(this)}
        />
      </div>
    )
  }
}

ReactDOM.render(
  <MyComponent/>,
  document.querySelector('#app')
);
```
获取`myInput`真实DOM的值：
```
var myInput = this.refs.myInput.props.value;
```
