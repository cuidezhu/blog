---
title: "迷你 Redux 实现"
date: 2018-07-14T20:04:01+08:00
draft: false
slug: "Mini-Redux-Implemention"
---

redux 主要由三部分组成：action, reduer, store，

## Action

Action 是用户自己定义的，用来描述发生了什么, action 的 type 字段是必须的，其它字段可以自己定义：

```js
const action = {
  type: 'ADD_TODO',
};
```

## Reducer

Reducer 也是由用户负责编写的，Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的，记住 actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return state + 1
    default:
      return state
  }
}
```

## Store

redux 只有一个单一的 Store, 用户通过 `createStore()` 来创建 store，`createStore()` 接受一个 reducer 函数作为参数, 我们使用 `combineReducers()` 将多个 reducer 合并成为一个。

`createStore()` 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。

```js
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

Store 有以下职责：

* 维持应用的 state；
* 提供 `getState()` 方法获取 state；
* 提供 `dispatch(action)` 方法更新 state；
* 通过 `subscribe(listener)` 注册监听器;
* 通过 `subscribe(listener)` 返回的函数注销监听器。

## 实现 mini-redux

下面我们来实现 redux 核心的 `createStore()` 函数

```js
export function createStore(reducer) {
  let currentState = {}
  let currentListeners = []

  function getState() {
    return currentState
  }

  function subscribe(listener) {
    if (typeof listener !== 'function') {
      throw new Error('Expected the listener to be a function.')
    }
    currentListeners.push(listener)
  }

  function dispatch(action) {
    currentState = reducer(currentState, action)
    currentListeners.forEach(v => v())

    return action
  }

  // type 定义为一个特殊的值，触发 reducer 的 default 分支来获取初始 state
  dispatch({type: '@@mini-redux/INIT'})

  return { getState, subscribe, dispatch}
}
```

然后我们实现 `bindActionCreators()`,这个函数首先负责把action创建函数用dispatch包一层，然后返回有相同keys的一个对象

```js
// bindActionCreators

function bindActionCreator(creator, dispatch) {
  return (...args) => dispatch(creator(...args))
}

export function bindActionCreators(creators, dispatch) {
  let bound = {}
  Object.keys(creators).forEach(v => {
    let creator = creators[v]
    bound[v] = bindActionCreator(creator, dispatch)
  })

  return bound
}
```


上面就把一个最简单的 Redux 给实现了。