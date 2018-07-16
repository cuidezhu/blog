---
title: "迷你 Redux 实现"
date: 2018-07-14T20:04:01+08:00
draft: false
slug: "Mini-Redux-Implemention"
---

```js
export function createStore(reducer) {
  let currentState = {}
  let currentListeners = []

  function getState() {
    return currentState
  }

  function subscribe(listener) {
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