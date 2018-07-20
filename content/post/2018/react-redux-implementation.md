---
title: "实现 react-redux"
date: 2018-07-18T20:45:52+08:00
draft: false
slug: "react-redux-implementation"
---

前面的文章 [迷你 Redux 实现](ijs.me/2018/07/14/mini-redux-implemention/) 我们实现了一个简单的 Redux, 那么我们怎么在 React 项目里使用我们自制的 Redux 呢？我们可以显式传递 Store，但更优雅的方式是使用 react-redux，这篇文章会实现我们自己的react-redux。

## 显式传递 Store

我们可以手动用 subscribe 订阅 ReactDOM.render 来将把 redux 和 react 结合起来，将 Store 集成到 UI 中最合乎直觉逻辑的做法是显式地将它作为属性在组件树中向下传递。例如下面的 index.js 文件中渲染一个 App 组件并传递 Store:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './components/App'
import { counter } from './index.redux'

const store = createStore(counter)

const render = () => 
  ReactDOM.render(
    <App store={store} />,
    document.getElementById('root')
  )

store.subscribe(render)

render()
```

在这个文件中，我们使用 counter 这个 reducer 处理函数来创建了一个 store，当 App 组件渲染完毕后，Store 会作为属性传递给它。每次 Store 发生变化后，render 函数就会被触发执行，这样就可以使用新的 State 数据高效地更新 UI 了。