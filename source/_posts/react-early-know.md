---
title: React 学习笔记
tags: react
abbrlink: 44509
date: 2016-10-09 20:17:35
---

React是Facebook和Instagram用来创建用户界面的JavaScript库。很多人将React认为是MVC中的V。 React的创建是为了解决一个问题：如何构建一个数据交互频繁的大型应用程序？为了实现这个目标，React运用了两个思想：

<!-- more -->

### 实时更新数据
React使得展现数据变得简单，并且当数据改变时，React能自动保持UI的更新。

### 构建通用组件
React旨在构建通用组件。试试想，写React代码就是在构建组件。因为组件式封装的，所以组件使得代码的复用性、测试性和关注分离变得简单。

### 基本概念
**React.js**
React.js 是 React 的核心库，在应用中必须先加载核心库。

**ReactDOM.js**
ReactDOM.js 是 React 的 DOM 渲染器，React 将核心库和渲染器分离开了，为了在 web 页面中显示开发的组件，需要调用 ReactDOM.render 方法， 第一个参数是 React 组件，第二个参数为 HTMLElement。

**JSX**
JSX 是 React 自定义的语法，最终 JSX 会转化为 JS 运行于页面当中。

**组件**
组件是 React 中的核心概念，页面当中的所有元素都是通过 React 组件来表达， 我们将要写的 React 代码绝大部分都是在做 React 组件的开发。

**VIRTUAL DOM**
React 抽象出来的虚拟 DOM 树，虚拟树是 React 高性能的关键。

**单向数据流：one-way reactive data flow**
React 应用的核心设计模式，数据流向自顶向下

### Hello World
创建一个简单的 `Hello World`，新建 index.html：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>React</title>
</head>
<body>

<div id="example"></div>

<script src="js/react.js"></script>
<script src="js/JSXTransformer.js"></script>
<script src="js/app.js" type="text/jsx"></script>
</body>
</html>
```
React独创了一种JS、CSS和HTML混写的JSX格式，可以通过在页面中引入JSXTransformer这个文件进行客户端的编译，不过还是推荐在服务端编译。
app.js :
```JS
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

React.render(
  <HelloMessage name="John" />,
  document.getElementById('container')
);
```
### React.createClass
用来创建一个组件类，编写React代码主要就是构建通用的组件。

### React.render
将React的模板转化为HTML，并插入到相应的DOM结构中。要注意的是，React的渲染函数并不是简单地把HTML元素复制到页面上，而是维护了一张Virtual Dom映射表。
