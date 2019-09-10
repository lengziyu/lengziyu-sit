---
title: 'React 数据流 Props 和 State [组件沟通]'
tags:
  - react
categories:
  - react
abbrlink: 932
date: 2016-10-11 18:50:20
---

组件沟通因为React的单向数据流方式会有所限制，下面述说组件之间的沟通方式。

<!-- more -->

可以分为以下 3 种：
- 【父组件】向【子组件】传值；
- 【子组件】向【父组件】传值；
- 没有任何嵌套关系的组件之间传值（PS：比如：兄弟组件之间传值）



### 【父组件】向【子组件】传值
父组件更新子组件状态，通过传递`props`，就可以了。例子如下：
```JS
// 父组件
var MyContainer = React.createClass({
  getInitialState: function () {
    return {
      checked: true
    };
  },
  render: function() {
    return (
      <ToggleButton text="Toggle me" checked={this.state.checked} />
    );
  }
});

// 子组件
var ToggleButton = React.createClass({
  render: function () {
    // 从【父组件】获取的值
    var checked = this.props.checked,
        text = this.props.text;

    return (
        <label>{text}: <input type="checkbox" checked={checked} /></label>
    );
  }
});
```
进一步讨论

如果组件嵌套层次太深，那么从外到内组件的交流成本就变得很高，通过 props 传递值的优势就不那么明显了。（PS：所以我建议尽可能的减少组件的层次，就像写 HTML 一样，简单清晰的结构更惹人爱）

```JS
// 父组件
var MyContainer = React.createClass({
  render: function() {
    return (
      <Intermediate text="where is my son?" />
    );
  }
});

// 子组件1：中间嵌套的组件
var Intermediate = React.createClass({
  render: function () {
    return (
      <Child text={this.props.text} />
    );
  }
});

// 子组件2：子组件1的子组件
var Child = React.createClass({
  render: function () {
    return (
      <span>{this.props.text}</span>
    );
  }
});
```
### 【子组件】向【父组件】传值
接下来，我们介绍【子组件】控制自己的 state 然后告诉【父组件】的点击状态，然后在【父组件】中展示出来。因此，我们添加一个 change 事件来做交互。
```JS
// 父组件
var MyContainer = React.createClass({
  getInitialState: function () {
    return {
      checked: false
    };
  },
  onChildChanged: function (newState) {
    this.setState({
      checked: newState
    });
  },
  render: function() {
    var isChecked = this.state.checked ? 'yes' : 'no';
    return (
      <div>
        <div>Are you checked: {isChecked}</div>
        <ToggleButton text="Toggle me"
          initialChecked={this.state.checked}
          callbackParent={this.onChildChanged}
          />
      </div>
    );
  }
});

// 子组件
var ToggleButton = React.createClass({
  getInitialState: function () {
    return {
      checked: this.props.initialChecked
    };
  },
  onTextChange: function () {
    var newState = !this.state.checked;
    this.setState({
      checked: newState
    });
    // 这里要注意：setState 是一个异步方法，所以需要操作缓存的当前值
    this.props.callbackParent(newState);
  },
  render: function () {
    // 从【父组件】获取的值
    var text = this.props.text;
    // 组件自身的状态数据
    var checked = this.state.checked;

    return (
        <label>{text}: <input type="checkbox" checked={checked} onChange={this.onTextChange} /></label>
    );
  }
});
```
这样做其实是依赖 props 来传递事件的引用，并通过回调的方式来实现的，这样实现不是特别好，但是在没有任何工具的情况下也是一种简单的实现方式

这里会出现一个我们在之前讨论的问题，就是组件有多层嵌套的情况下，你必须要一次传入回调函数给 props 来实现子组件向父组件传值或者操作。

### 兄弟组件之间传值
当两个组件有相同的父组件时，就称为兄弟组件（堂兄也算的）。按照React单向数据流方式，我们需要借助父组件进行传递，通过父组件回调函数改变兄弟组件的`props`。

**方式一**
通过`props`传递父组件回调函数。
```JS
class Brother1 extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }

  render(){
    return (
      <div>
        <button onClick={this.props.refresh}>
            更新兄弟组件
        </button>
      </div>
    )
  }
}
class Brother2 extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }

  render(){
    return (
      <div>
         {this.props.text || "兄弟组件未更新"}
      </div>
    )
  }
}
class Parent extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }
  refresh(){
    return (e)=>{
      this.setState({
        text: "兄弟组件沟通成功",
      })
    }
  }
  render(){
    return (
      <div>
        <h2>兄弟组件沟通</h2>
        <Brother1 refresh={this.refresh()}/>
        <Brother2 text={this.state.text}/>
      </div>
    )
  }
}
```
**方式二**
但是如果组件层次太深，上面的兄弟组件沟通方式就效率低了（不建议组件层次太深）。
React提供了一种上下文方式（挺方便的），可以让子组件直接访问祖先的数据或函数，无需从祖先组件一层层地传递数据到子组件中。
```JS
class Brother1 extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }

  render(){

    return (
      <div>
        <button onClick={this.context.refresh}>
            更新兄弟组件
        </button>
      </div>
    )
  }
}
Brother1.contextTypes = {
  refresh: React.PropTypes.any
}
class Brother2 extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }

  render(){
    return (
      <div>
         {this.context.text || "兄弟组件未更新"}
      </div>
    )
  }
}
Brother2.contextTypes = {
  text: React.PropTypes.any
}
class Parent extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }

  getChildContext(){
    return {
      refresh: this.refresh(),
          text: this.state.text,
      }
    }

  refresh(){
    return (e)=>{
      this.setState({
        text: "兄弟组件沟通成功",
      })
    }
  }
  render(){
    return (
      <div>
        <h2>兄弟组件沟通</h2>
        <Brother1 />
        <Brother2 text={this.state.text}/>
      </div>
    )
  }
}
Parent.childContextTypes = {
  refresh: React.PropTypes.any,
  text: React.PropTypes.any,
}
```
### 全局事件
官网中提到可以使用全局事件来进行组件间的通信，官网推荐Flux（Facebook官方出的），还有Relay、Redux、trandux等第三方类库。这些框架思想都一致，都是统一管理组件state变化情况，达到数据可控目的。

### 总结
简单的组件交流我们可以使用上面非全局事件的简单方式，但是当项目复杂，组件间层次越来越深，上面的交流方式就不太合适（当然还是要用到的，简单的交流）。强烈建议使用Flux、Relay、Redux、trandux等类库其中一种，这些类库不只适合React，像Angular等都可以使用。

### 参考
- [ReactJS组件间沟通的一些方法](http://www.alloyteam.com/2016/01/some-methods-of-reactjs-communication-between-components/)
- [React 数据流管理架构之 Redux 介绍](http://www.alloyteam.com/2015/09/react-redux/)
