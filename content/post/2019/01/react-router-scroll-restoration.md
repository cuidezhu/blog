---
title: "React Router 组件切换时滚动条重置到页面顶部"
date: 2019-01-21T18:09:15+08:00
draft: false
slug: "react-router-scroll-restoration"
---

我们在使用 React Router V4 时可以发现在路由切换时滚动条的位置没有变化，进入一个新的页面后滚动条还是在上页时的位置，这显然不符合我们阅读网页的习惯。

在早期的 React Router 版本是提供开箱即用的滚动到顶部重置支持的。因为现在浏览器开始支持了 HTML5 引入的 history.pushState() 方法，浏览器开始处理默认的滚动条状态，并且不同的应用对是否滚动到顶部有不同的需求。所以我们需要自己来根据需求来实现。

现在我们来实现在路由切换时滚动条重置到页面顶部。

我们在项目中的工具文件夹里新建一个 `/src/utils/ScrollToTop.js`

```js
import React, { Component } from 'react'
import { withRouter } from 'react-router-dom'

class ScrollToTop extends Component {
  componentDidUpdate(prevProps) {
    if (this.props.location.pathname !== prevProps.location.pathname) {
      window.scrollTo(0, 0);
    }
  }

  render() {
    return this.props.children;
  }
}

export default withRouter(ScrollToTop);
```

然后在我们的路由文件里引入 `ScrollToTop` 组件，并用 `ScrollToTop` 组件把我们的路由包裹一层，这样就实现了当路由切换时滚动条自动重置到页面顶部。

