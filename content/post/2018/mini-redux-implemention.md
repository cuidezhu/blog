---
title: "迷你 Redux 实现"
date: 2018-07-14T20:04:01+08:00
draft: false
slug: "Mini-Redux-Implemention"
---

redux 主要由三部分组成：action, reduer, store，

## action

action 是用户自己定义的，用来描述发生了什么, action 的 type 字段是必须的，其它字段可以自己定义：

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

redux 只有一个单一的 Store 通过 `createStore()` 来创建创建 store，

Store 有以下职责：

* 维持应用的 state；
* 提供 getState() 方法获取 state；
* 提供 dispatch(action) 方法更新 state；
* 通过 subscribe(listener) 注册监听器;
* 通过 subscribe(listener) 返回的函数注销监听器。

## 实现 mini-redux

下面我们来实现 redux 核心的 createStore() 函数

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

上面就把一个最简单的 Redux 给实现了。