---
title: "React 使用 Promise 等待外部脚本加载完成再执行代码"
date: 2018-06-05T16:09:35+08:00
draft: false
slug: "React-Load-Script"
---


有时我们做项目时，需要引入外部的脚本，有些是需要等脚本加载完才可以执行相关的代码。我们在 React 项目中，可以用 `Promise` 来做这件事儿。

首先需要如下加载脚本的函数：

```js
function loadScript(src) {
  return new Promise(resolve => {
    let tag = document.createElement("script")
    tag.async = true
    tag.src = src

    document.body.appendChild(tag)

    tag.addEventListener("load", function() {
      resolve()
    })
  })
}
```

当一个资源及其依赖资源已完成加载时，将触发 `load` 事件。

然后我们在组件的 `componentDidMount()` 里调用这个函数：

```js
componentDidMount() {

  Promise.all([
    loadScript("/a.js"),
    loadScript("/b.js"),
    loadScript("/c.js")
  ]).then(() => {
    // your code goes here
  }
}
```

这样我们就实现了先加载完 `a.js`, `b.js`, `c.js` 这三个脚本再执行相关代码，这里我们需要加载三个脚本，用到了 `Promise.all(iterable)`, 这个方法返回一个新的 promise 对象，该 promise 对象在 iterable 参数对象里所有的 promise 对象都成功的时候才会触发成功，一旦有任何一个 iterable 里面的 promise 对象失败则立即触发该 promise 对象的失败。

如果只加载一个脚本，可以这样：

```js
componentDidMount() {

  loadScript("/a.js").then(() => {
    // your code goes here
  }
}
```



