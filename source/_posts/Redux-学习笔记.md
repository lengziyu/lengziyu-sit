---
title: Redux 简明教程（转）
tags: redux
categories: redux
abbrlink: 51341
date: 2016-10-13 20:26:49
---
Redux 是 JavaScript 状态容器，提供可预测化的状态管理。

可以让你构建一致化的应用，运行于不同的环境（客户端、服务器、原生应用），并且易于测试。不仅于此，它还提供 超爽的开发体验，比如有一个[时间旅行调试器可以编辑后实时预览](https://github.com/gaearon/redux-devtools)。
<!--more-->

Redux 除了和 React 一起用外，还支持其它界面库。
它体小精悍（只有2kB）且没有任何依赖。

## 启示
Redux 由 Flux 演变而来，但受 Elm 的启发，避开了 Flux 的复杂性。

## Store
首先要区分 `store` 和 `state`
`state` 是应用的状态，一般本质上是一个普通对象
例如，我们有一个 Web APP，包含 计数器 和 待办事项 两大功能
那么我们可以为该应用设计出对应的存储数据结构（应用初始状态）：
```
/** 应用初始 state，本代码块记为 code-1 **/
{
  counter: 0,
  todos: []
}
```
`store` 是应用状态 `state` 的管理者，包含下列四个函数：
- getState() # 获取整个 state
- dispatch(action) # ※ 触发 state 改变的【唯一途径】※
- subscribe(listener) # 您可以理解成是 DOM 中的 addEventListener
- replaceReducer(nextReducer) # 一般在 Webpack Code-Splitting 按需加载的时候用

二者的关系是：`state = store.getState()`
Redux 规定，一个应用只应有一个单一的 `store`，其管理着唯一的应用状态 `state`
Redux 还规定，不能直接修改应用的状态 `state`，也就是说，下面的行为是不允许的：

```
var state = store.getState()
state.counter = state.counter + 1 // 禁止在业务逻辑中直接修改 state
```
若要改变 `state`，必须 `dispatch` 一个 `action`，这是修改应用状态的不二法门
<div class="tip">
现在您只需要记住 action 只是一个包含 type 属性的普通对象即可
例如 { type: 'INCREMENT' }
</div>
上面提到，`state` 是通过 `store.getState()` 获取，那么 `store` 又是怎么来的呢？
想生成一个 `store`，我们需要调用 Redux 的 `createStore`：
```
import { createStore } from 'redux'
...
const store = createStore(reducer, initialState) // store 是靠传入 reducer 生成的哦！
```
<div class="tip">
现在您只需要记住 reducer 是一个 函数，负责更新并返回一个新的 state
而 initialState 主要用于前后端同构的数据同步（详情请关注 React 服务端渲染）
</div>

## Action
上面提到，`action`（动作）实质上是包含 `type` 属性的普通对象，这个 `type` 是我们实现用户行为追踪的关键
例如，增加一个待办事项 的 `action` 可能是像下面一样：
```
/** 本代码块记为 code-2 **/
{
  type: 'ADD_TODO',
  payload: {
    id: 1,
    content: '待办事项1',
    completed: false
  }
}
```
当然，action 的形式是多种多样的，唯一的约束仅仅就是包含一个 type 属性罢了
也就是说，下面这些 action 都是合法的：
```
/** 如下都是合法的，但就是不够规范 **/
{
  type: 'ADD_TODO',
  id: 1,
  content: '待办事项1',
  completed: false
}

{
  type: 'ADD_TODO',
  abcdefg: {
    id: 1,
    content: '待办事项1',
    completed: false
  }
}
```
**虽说没有约束，但最好还是遵循[规范](https://github.com/acdlite/flux-standard-action)**
如果需要新增一个代办事项，实际上就是将 code-2 中的 payload “写入” 到 state.todos 数组中（如何“写入”？在此留个悬念）：
```
/** 本代码块记为 code-3 **/
{
  counter: 0,
  todos: [{
    id: 1,
    content: '待办事项1',
    completed: false
  }]
}
```
刨根问底，action 是谁生成的呢？

## Action Creator
<div class="tip">
Action Creator 可以是同步的，也可以是异步的
</div>
顾名思义，Action Creator 是 action 的创造者，本质上就是一个函数，返回值是一个 action（对象）
例如下面就是一个 “新增一个待办事项” 的 Action Creator：
```
/** 本代码块记为 code-4 **/
var id = 1
function addTodo(content) {
  return {
    type: 'ADD_TODO',
    payload: {
      id: id++,
      content: content, // 待办事项内容
      completed: false  // 是否完成的标识
    }
  }
}
```
将该函数应用到一个表单（假设 store 为全局变量，并引入了 jQuery ）：
```
<--! 本代码块记为 code-5 -->
<input type="text" id="todoInput" />
<button id="btn">提交</button>

<script>
$('#btn').on('click', function() {
  var content = $('#todoInput').val() // 获取输入框的值
  var action = addTodo(content) // 执行 Action Creator 获得 action
  store.dispatch(action) // 改变 state 的不二法门：dispatch 一个 action！！！
})
</script>
```
在输入框中输入 “待办事项2” 后，点击一下提交按钮，我们的 state 就变成了：
```
/** 本代码块记为 code-6 **/
{
  counter: 0,
  todos: [{
    id: 1,
    content: '待办事项1',
    completed: false
  }, {
    id: 2,
    content: '待办事项2',
    completed: false
  }]
}
```
**通俗点讲，Action Creator 用于绑定到用户的操作（点击按钮等），其返回值 action 用于之后的 dispatch(action)**
刚刚提到过，action 明明就没有强制的规范，为什么 store.dispatch(action) 之后，
Redux 会明确知道是提取 action.payload，并且是对应写入到 state.todos 数组中？
又是谁负责“写入”的呢？悬念即将揭晓...

## Reducer
**Reducer 必须是同步的纯函数**
用户每次 dispatch(action) 后，都会触发 reducer 的执行
reducer 的实质是一个函数，根据 action.type 来更新 state 并返回 nextState
最后会用 reducer 的返回值 nextState 完全替换掉原来的 state
<div class="tip">
注意：上面的这个 “更新” 并不是指 reducer 可以直接对 state 进行修改
Redux 规定，须先复制一份 state，在副本 nextState 上进行修改操作
例如，可以使用 lodash 的 cloneDeep，也可以使用 Object.assign / map / filter/ ... 等返回副本的函数
</div>
在上面 Action Creator 中提到的 待办事项的 reducer 大概是长这个样子 (为了容易理解，在此不使用 ES6 / Immutable.js)：
```
/** 本代码块记为 code-7 **/
var initState = {
  counter: 0,
  todos: []
}

function reducer(state, action) {
  // ※ 应用的初始状态是在第一次执行 reducer 时设置的 ※
  if (!state) state = initState

  switch (action.type) {
    case 'ADD_TODO':
      var nextState = _.cloneDeep(state) // 用到了 lodash 的深克隆
      nextState.todos.push(action.payload)
      return nextState

    default:
    // 由于 nextState 会把原 state 整个替换掉
    // 若无修改，必须返回原 state（否则就是 undefined）
      return state
  }
}
```
通俗点讲，就是 reducer 返回啥，state 就被替换成啥

## 总结
- store 由 Redux 的 createStore(reducer) 生成
- state 通过 store.getState() 获取，本质上一般是一个存储着整个应用状态的对象
- action 本质上是一个包含 type 属性的普通对象，由 Action Creator (函数) 产生
- 改变 state 必须 dispatch 一个 action
- reducer 本质上是根据 action.type 来更新 state 并返回 nextState 的函数
- reducer 必须返回值，否则 nextState 即为 undefined
- 实际上，state 就是所有 reducer 返回值的汇总（本教程只有一个 reducer，主要是应用场景比较简单）

**Action Creator => action => store.dispatch(action) => reducer(state, action) => 原 state state = nextState**

原文地址：[Redux 简明教程](https://github.com/kenberkeley/redux-simple-tutorial)
