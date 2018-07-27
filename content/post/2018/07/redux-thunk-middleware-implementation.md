---
title: "redux-thunk 中间件的实现"
date: 2018-07-22T22:05:08+08:00
draft: false
slug: "redux-thunk-middleware-implementation"
---

## 同步中间件

![redux-middleware-sync](static/img/2018/07/redux-middleware-sync.png)

通过 redux 暴露出的 applyMiddleware，我们可以得到一个新的 dispatch.
所以 applyMiddleware 的实际效果可以看做是：dispatch => newdispatch

## 异步中间件

![redux-middleware-async](static/img/2018/07/redux-middleware-async.png)

对于异步的情况，我们拿 redux-thunk 为例，它会对传入的 action 进行判断，如果是 function 的话。则去把我们的 action 执行，这个执行的就是我们的 delay 代码。然后在最里面执行 dispatch()。所以我们的箭头又重新回到最上面，这一次就相当于同步的使用了。

redux-thunk 允许我们的 action 创建函数 return 一个函数而不是一个 action, 经常用来延迟 dispatch 某个 action。redux-thunk 本身代码非常简单，下面我们来实现一下：

```js
const thunk = ({dispatch, getState}) => next => action => {
  // 如果是函数，执行一下，参数是dispatch和getState
  if (typeof action === 'function') {
    return action(dispatch, getState)
  }
  // 默认什么都没干
  return next(action)
}

export default thunk
```