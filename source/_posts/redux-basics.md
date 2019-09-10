---
title: redux 基础
tags: redux
abbrlink: 64007
date: 2016-12-08 22:01:25
categories:
---
### Store
Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store。
Redux 提供createStore这个函数，用来生成 Store。
<!--more-->
```
import { createStore } from 'redux';
const store = createStore(fn);
```
上面createStore函数接受另一个函数作为参数，返回新生成的 Store 对象。


### State
Store对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。
当前时刻的 State，可以通过store.getState()拿到。
```
const state = store.getState();
```
Redux 规定， 一个 State 对应一个 View。只要 State 相同，View 就相同。你知道 State，就知道 View 是什么样，反之亦然。


### Action
```
Action 是一个对象。其中的type属性是必须的，表示 Action 的名称
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```
Action 的名称是ADD_TODO，它携带的信息是字符串Learn Redux
可以这样理解，Action 描述当前发生的事情。改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。


### Action Creator
可以定义一个函数来生成Action，这个函数叫Action Creator
```
const ADD_TODO = '添加 TODO';
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
const action = addTodo('Learn Redux');
```
上面代码中，add Todo就是一个Action Creator


### store.dispatch()
store.dispatch()是view发出Action的唯一方法
```
import { createStore } from 'redux';
const store = createStore(fn)

 store.dispatch({
     type: 'ADD_TODO',
     payload: 'Learn Redux'
 })
```
 结合
```
store.dispatch(addTodo('Learn Redux'))
```


### Reducer
Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会有变化。这种 State 的计算过程叫做 Reducer
是一个函数，它能接受 Action 和 当前 State 作为参数，返回新的 State。
```
const reducer = function(action, state) {
  return newState;
}
```
store.dispatch 会触发reducer自动执行，做法就是在生成store的时候，将reducer传入createStore方法。
```
const store = createStore(reducer);
```


### store.subscribe()
Store 允许使用 store.subcribe() 设置为监听函数，一旦 State 发生变化，就自动执行这个函数。
```
import { createStore } from 'redux';
const store = createStore(reducer);
store.subscribe(listener);
```
只要把view的更新函数放入listener，就会实现view的自动渲染。

store.subscribe 方法返回一个函数，调用这个函数就可以解除
```
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
 )
unsubscribe()
```

### Store 的实现
提供了三个方法
- store.getState()
- store.dispatch()
- store.subscrobe()
```
import { createStore } from 'redux';
let { getState, dispatch, subscribe } = createStore(reducer);
```

### Reducer 拆分
Redux 提供了 combineReducers 方法拆分 Reducer
```
import { combineReducers } from 'redux';
const reduer = combineReducers({
  a: doSomethingsWidthA,
  b: processB,
  c: c
})
```
把子reducer放在一个文件里，然后统一引入
```
import { createStore } from 'redux'
import * as reducers from './reducers'
const reducer = combineReducers(reducers)
```
