---
title: 'React 数据流 Props 和 State [基础介绍]'
tags:
  - react
categories:
  - react
abbrlink: 1975
date: 2016-10-10 19:17:09
---

使用React我们首先要知道如何传递数据，组件如何沟通，才能展示我们想要的数据。下面的列子都是使用ES6语法，不懂的同学需要先学习ES6语法。

<!-- more -->

### 数据流
React是单向数据流，从父节点传递到子节点（通过`props`）。如果顶层的某个`props`改变了，React会重渲染所有的子节点（未做性能优化）。严格意义上React只提供，也强烈建议使用这种数据交流方式。

### Props
`props`是`property`的缩写，可以理解为HTML标签的`attribute`。请把`props`当做只读的（不可以使用`this.props`直接修改`props`），`props`是用于整个组件树中传递数据和配置。在当前组件访问`props`，使用`this.props`。
```JS
var HelloWorld = React.createClass({
    render: function () {
        return (
            <div data-title={this.props.title}>{this.props.content}</div>
        )
    }
});

React.render(
    <HelloWorld title="this is title" content="this is content"/>,
    document.body
);
```

### state
每个组件都有属于自己的`state`，`state`和`props`的区别在于前者之只存在于组件内部，只能从当前组件调用`this.setState`修改`state`值（不可以直接修改`this.state`）。一般我们更新子组件都是通过改变`state`值，更新新子组件的`props`值从而达到更新。

那如何设置默认state?
```JS
//React提供的crateClass创建方式
var Component = React.createClass({
  getInitialState(){
    return {
      //这里设置初始state值
    }
  }
})
//ES6 && ES7
class Component {
  constructor(){
    this.state = {}//在ES6中的构造函数中初始化，可以之直接赋值，在其他方法中，只能使用this.setState
  }
  ...
}
```
再看一个例子，点击按钮，切换按钮的颜色：
```
var ColorButton = React.createClass({
    getInitialState: function () {
        return {bColor: 'green'};
    },
    render: function () {
        return (
            <button onClick={this.handleClick} style={{backgroundColor: this.state.bColor}}>click</button>
        )
    },
    handleClick: function (event) {
        this.setState({bColor: this.state.bColor === 'green' ? 'red' : 'green'});
    }
});

React.render(
    <ColorButton />,
    document.body
);
```
handleClick是用来处理我们点击事件的。

### state工作原理
通过调用setState(data, callback)方法，改变状态，就会触发React更新UI。大部分情况下，我们不需要提供callback函数。React会自动的帮我们更新UI。

### 什么样的组件该有state
大部分的组件应该从props属性中获取数据并渲染。但有的时候组件得相应用户输入，同服务器交互，这些情况下会用到state。React的官方说法是：**尽可能的保持你的组件无状态化**。为了实现这个目标，得保持你的状态同业务逻辑分离，并减少冗余信息，尽可能保持组件的单一职责。

React官方推荐的一种模式就是：构建几个无状态的组件用来渲染数据，在这些之上构建一个有状态的组件同用户和服务交互，数据通过props传递给无状态的组件。我的理解大概就是这样：
```JS
var RenderComponent = React.createClass({
    render: function () {
        return (
            <ul>
                {
                    this.props['data-list'].map(function (item) {
                        return (<li>{item}</li>)
                    })
                }
            </ul>
        )
    }
});


var StateComponent = React.createClass({
    getInitialState: function () {
        return {list: ['xxx', 'yyy']};
    },
    render: function () {
        return (
            <div>
                <button onClick={this.handleClick}>click</button>
                <RenderComponent data-list={this.state.list}/>
            </div>

        )
    },
    handleClick: function () {
        this.setState({list: [1, 2, 3]});
    }
});

React.render(
    <StateComponent />,
    document.body
);
```
UI交互会导致改变的数据。
state不应包含什么样的数据：1.计算过的数据；2.组件；3.从props复制的数据。
state应保含最原始的数据，比如说时间，格式化应该交给展现层去做。组件应在render方法里控制。

### props和state使用方式
<div class="tip">
尽可能使用props当做数据源，state用来存放状态值（简单的数据），如复选框、下拉菜单等。
</div>
