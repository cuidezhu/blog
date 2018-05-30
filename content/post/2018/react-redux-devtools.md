---
title: "React Redux Devtools 导致 Chrome 之外的浏览器报错处理"
date: 2018-05-30T08:21:44+08:00
draft: false
slug: "React-Redux-Devtools"
---

用 Redux 配合 React 开发项目时遇到一个奇怪的问题，项目只能在 Chrome 浏览器下正确运行，其它的浏览器都会报如下错误：`TypeError: undefined is not an object (evaluating 'b.apply')`

![redux-devtools-error](/img/2018/redux-devtools-error.png)

猜测可能是由 Redux 引起的，于是搜索一番，找到了这个 [Github Issues](https://github.com/reduxjs/redux/issues/2033), 原来是因为只有我的 Chrome 浏览器装了 Redux DevTools 这个插件，而其它浏览器都没装，所以下面的代码会报错，

```js
const store = createStore(reducers, compose(
  applyMiddleware(thunk),
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
))
```

正确的做法是：

```js
const store = createStore(reducers, compose(
  applyMiddleware(thunk),
  window.devToolsExtension ? window.devToolsExtension() : f=>f
))
```

这样项目就不会在 Chrome 之外的浏览器报错了。