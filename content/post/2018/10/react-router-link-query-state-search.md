---
title: "在 React Router 的 Link 中用 query state 和 search 三种方式传参数的差异"
date: 2018-10-19T16:29:45+08:00
draft: false
slug: "react-router-link-query-state-search"
---

我们在页面间跳转时，经常需要传一些参数到下一个页面，我们可以在 Link 中携带一些参数到下一个页面，发现网上的一些文章有些错误，官方文档也没有很详细的说明，根据尝试，做出以下区分：

## query

```js
<Link to={{pathname: `/project`, query: function a() {return 'name=test'}}}>项目</Link>
```

query 支持所有的 JS 基本数据类型：字符串（String）、数字(Number)、布尔(Boolean)、空值（Null）、未定义（Undefined）、Symbol和引用数据类型：对象(Object)、数组(Array)、函数(Function)，在新页面刷新后，传来的数据不复存在，不用 query 这个标识符随便起一个名字（除了 state 和 search）和 query 作用完全相同，query 不会把数据加到 URL 上。

在新页面可以通过 `this.props.location.query` 拿到 `function a() {return 'name=test'}`。

## state

```js
<Link to={{pathname: `/project`, state:{name: 'test'}}}>项目</Link>
```

state 支持除 Symbol 和函数之外的所有数据类型包括基本数据类型：字符串（String）、数字(Number)、布尔(Boolean)、空值（Null）、未定义（Undefined）和引用数据类型：对象(Object)、数组(Array)，在新页面刷新后数据还存在，state 不会把数据加到 URL 上。

在新页面可以通过 `this.props.location.state` 拿到 `{name: 'test'}`。

## search

```js
<Link to={{pathname: `/project`, search: 'name=test'}}>项目</Link>
```

search 只支持字符串，在新页面刷新后数据还存在，会把数据加到 URL 上，URL 上的数据前面会加 `?`，类似于 `https://a.com/project?name=test`。

在新页面可以通过 `this.props.location.search` 拿到 `'name=test'`。