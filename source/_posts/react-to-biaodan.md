---
title: React 笔记之 表单
tags: react
categories: react
abbrlink: 13509
date: 2016-10-18 21:48:08
---

诸如： `<input>`、`<textarea>`、`<option>` 这样的表单组件不同于其他组件，因为他们可以通过用户交互发生变化。这些组件提供的界面使响应用户交互的表单数据处理更加容易。
<!--more-->
## 交互属性
在 HTML 中`<textarea>`的值通过子节点设置；在 React 中则应该使用 value 代替。
表单组件可以通过onChange回调函数来监听组件变化。当用户做出以下交互时,onChange执行并通过浏览器做出响应：
- `<input>`或 `<textarea>` 的 `value` 发生变化时。
- `<input>`的 `checked` 状态改变时。
- `<option>` 的 `selected` 状态改变时。
和所有 DOM 事件一样，所有的 HTML 原生组件都支持 onChange 属性，而且可以用来监听冒泡的 change 事件.

<div class="tip">
对于`<input>`和`<textarea>`，onChange应当被用于取代DOM内置的onInput事件处理
</div>

## 不可控组件和可控组件
**可控组件**
设置了 `value` 的 `<input>` 是一个受限组件。 对于受限的 `<input>`，渲染出来的 HTML 元素始终保持 `value` 属性的值。
```
render: function() {
    return <input type="text" value="Hello!"/>;
 }
```
上面的代码将渲染出一个值为 Hello! 的 input 元素。用户在渲染出来的元素里输入任何值都不起作用，因为 React 已经赋值为 Hello!。如果想响应更新用户输入的值，就得使用 onChange 事件
```
getInitialState: function() {
    return {value: 'Hello!'};
 },
 handleChange: function(event) {
    this.setState({value: event.target.value});
 },
 render: function() {
    var value = this.state.value;
    return <input type="text" value={value} onChange={this.handleChange} />;
 }
```
**Default Value**
初始值是状态中的value。如果要取数据，可直接使用 `var inputValue = this.state.value`。
```
render: function() {
   return <input type="text" defaultValue={this.state.value}/>;
}
```
<div class="tip">
一个可控组件并不保持自己的原始状态；组件的呈现完全基于属性。
</div>
**实例**
```
var Kekong = React.creatClass({
    getInitialState:function(){
        return {
            dada:'shuaige'
        }
    },
    handleChange:function(e){
        this.setState({
            dada:e.target.value
        });
    },
    submitHandler:function(e){
        e.preventDefault();
        alert(this.state.dada);
    },
    render:function(){
        return <form onSubmit={this.submitHandler}>
            <input type="text" value={this.state.dada} onChange={this.handleChange} />
            <button type="submit">speak</button>
        </form>;
    }
});
ReactDOM.render(<Kekong />,document.body);
```
**不可控组件**
没有设置value(或者设为null) 的`<input>`组件是一个不可控组件。这样的话，组件中的数据和state中的数据并不对应，可以说，组件的数据不可控。
```
render: function() {
    return <input type="text" />;
 }
```
上面的代码将渲染出一个空值的输入框，用户输入将立即反应到元素上。和受限元素一样，使用 onChange 事件可以监听值的变化。
**Default Value**
如果想给组件设置一个非空的初始值，可以使用 defaultValue 属性。 数据在这里并没有存贮在状态中，而是写在input中。
```
render: function() {
   return <input type="text" defaultValue="Hello!" />;
}
```
如果要拿到input中的value，需先拿到其DOM节点，然后获取其value值
```
var inputValue = React.findDOMNode(this.refs.input).value
```
上面的代码渲染出来的元素和受限组件一样有一个初始值，但这个值用户可以改变并会反应到界面上。
同样，`<input type="checkbox">`和`<input type="radio">`支持defaultChecked属性，`<select>`支持设置defaultValue。
`defaultValue`和`defaultChecked`属性只能在初始的render函数中使用，如果你要在随后的render函数中更新value值，你需要使用可控组件。

**实例**
```
var UnKekong = React.creatClass({
    submitHandler:function(e){
        e.preventDefault();
        var helloUnke = React.findDOMNode(this.refs.helloUnke).value;
        alert(helloUnke);
    },
    render:function(){
        return <form onsubmit={this.submitHandler}>
            <input ref="helloUnke" type="text" defaultValue="Dada shuaige" />
            <button type="submit">speak</button>
        </form>;
    }
})
React.render(<Unkekong />,document.body);
```
**Checkbox和Radio的潜在问题**
<div class="tip">
注意，在试图改变正常处理Checkbox和Radio input时，React用一个click事件来代替change事件。大多数情况下，这种行为与预期相同，除了调用preventDefault时。preventDefault从视觉上阻止浏览器更新input，即使checked被触发。它可以在移除调用preventDefault与用setTimeout来切换checked中起作用。
</div>
**Why use Controlled Components**
组件可控的优点：
- 符合React的数据流，单向数据流，从state流向render输出的结果。
- 数据存贮在state中，便于使用。
- 便于对数据进行处理


## 表单元素

- `<label htmlFor="name">Name</label>`
- 要注意for是js关键字，要写成htmlFor。具体JSX语法在之间笔记中有介绍，传送门：React.js学习笔记之JSX解读。现在多数提示用input的placeholder属性替代。
- `<input type="" onChange={this.handleChange}/>`
- `<textarea onChange={this.handleChange}/>`
- `<select onChange={this.handleChange}><option></option></select>`

**实例**
[这是一个demo传送门](https://github.com/Xiaoxianrou/Blog/tree/master/2016.03/React-Demo/demo3)
```
var MyForm = React.createClass({
getInitialState:function(){
    return {
        username:'',
        gender:'man',
        checked:true
    };
},
handleUsernameChange:function(e){
    this.setState({
        username:e.target.value
    });
},
handlerGenderChange:function(e){
    this.setState({
        gender:e.target.value
    });
},
handleCheckedChange:function(e){
    this.setState({
        checked:e.target.checked
    });
},
submitHandler:function (e) {
    e.preventDefault();
    console.log(this.state);
},
render:function () {
    return <form onSubmit={this.submitHandler}>
        <label htmlFor="username">请输入用户名</label>
        <input type="text" onChange={this.handleUsernameChange} value={this.state.username} id="username"/>
        <br/>
        <select onChange={this.handlerGenderChange} value={this.state.gender}>
            <option value="man">男</option>
            <option value="woman">女</option>
        </select>
        <br/>
        <label htmlFor="checkbox">大大是帅哥吗</label>
        <input type="checkbox" value="大大是帅哥" checked={this.state.checked} onChange={this.handleCheckedChange} id="checkbox"/>
        <button type="submit">提交</button>
    </form>
}
});
ReactDOM.render(<MyForm />,document.getElementById('reactDemo'));
```

## 事件处理函数

```
onChange={this.handleChange}
```
若有多个元素要运用事件处理函数，常规的方法是编写多个onChange事件。这么写的话会导致代码维护比较困难并且也非常冗余。更好的做法是把事件处理函数编写为一个。可以采用bind复用和name复用这两种方法。

**bind复用**
```
handleChange:function(name,event){
    ......
}
onChagne={this.handleChange.bind(this,'input')}
```
书写简单，但需要对bind()机制熟悉，性能相对要好。

**实例**
[这是一个demo传送门](https://github.com/Xiaoxianrou/Blog/tree/master/2016.03/React-Demo/demo3)
```
var MyForm = React.createClass({
    getInitialState:function(){
        return {
            username:'',
            gender:'man',
            checked:true
        };
    },
    handleChange:function(name,event){
        var newState={};
        newState[name]=name=="checked"?event.target.checked:event.target.value;
        this.setState(newState);
    },
    submitHandler:function (e) {
        e.preventDefault();
        console.log(this.state);
    },
    render:function () {
        return <form onSubmit={this.submitHandler}>
            <label htmlFor="username">请输入用户名</label>
            <input type="text" onChange={this.handleChange.bind(this,"username")} value={this.state.username} id="username"/>
            <br/>
            <select onChange={this.handleChange.bind(this,"gender")} value={this.state.gender}>
                <option value="man">男</option>
                <option value="woman">女</option>
            </select>
            <br/>
            <label htmlFor="checkbox">大大是帅哥吗</label>
            <input type="checkbox" value="大大是帅哥" checked={this.state.checked} onChange={this.handleChange.bind(this,"checked")} id="checkbox"/>
            <button type="submit">提交</button>
        </form>
    }
});
ReactDOM.render(<MyForm />,document.getElementById('reactDemo'));
```
**name复用**
```
handleChange:function(event){
    var name = event.target.name
}
onChange={this.handleChange}
```
相比Bind写法会少一些参数，在函数中需要读取表单的name值，需要添加name属性。

**实例**
```
var MyForm = React.createClass({
    getInitialState:function(){
        return {
            username:'',
            gender:'man',
            checked:true
        };
    },
    handleChange:function(event){
        var newState={};
        newState[event.target.name]=event.target.name=="checked"?event.target.checked:event.target.value;
        this.setState(newState);
    },
    submitHandler:function (e) {
        e.preventDefault();
        console.log(this.state);
    },
    render:function () {
        return <form onSubmit={this.submitHandler}>
            <label htmlFor="username">请输入用户名</label>
            <input type="text" name="username" onChange={this.handleChange} value={this.state.username} id="username"/>
            <br/>
            <select name="gender" onChange={this.handleChange} value={this.state.gender}>
                <option value="man">男</option>
                <option value="woman">女</option>
            </select>
            <br/>
            <label htmlFor="checkbox">大大是帅哥吗</label>
            <input type="checkbox" value="大大是帅哥" checked={this.state.checked} onChange={this.handleChange} name="checked" id="checkbox"/>
            <button type="submit">提交</button>
        </form>
    }
});
ReactDOM.render(<MyForm />,document.getElementById('reactDemo'));
```

## 自定义表单组件
自定义表单组件能让我们更好的使用组件，让我们更好的开发网页。

**why 自定义表单组件？**
自定义表单组件的原因：
- 内因：表单本身具备特殊性：样式统一、信息内聚、行为固定
- 外因：本质上是组件的嵌套，组织和管理组件的一种方式
