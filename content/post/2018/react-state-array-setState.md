---
title: "当React State 为 Array 时如何 setState"
date: 2018-05-08T13:14:20+08:00
draft: false
slug: "React-State-Array-setState"
---

某个数据类型为 Array 的 state，这里我们假设这个 state 名为 test，然后我们在组件的 constructor 里初始化这个 state:

```js
constructor(props) {
  super(props)
  this.state = {
    test: []
  }
}
```

数据使用 WebSocket 通信，当来了新数据时，比如我们接收到的数据为 data, data里有一个或者多个对象的数组，类似于这样 `[{},{}]`, 我们想把data里的数据存到 `this.state.test` 数组的头部，该怎么做呢，我们不能直接用 `unshift` 操作，因为 `setState` 才会触发 `render()` 函数重新渲染UI。

也不能这样做：

```js
this.setState({
  test: this.state.test.unshift(...data)
})
```

因为 `unshift` 返回的是新的长度，所以上面的做法是错误的。

正确的做法是：

```js
let newTest = this.state.test.unshift(...data)

this.setState({
  test: newTest
})
```

也可以使用扩展运算符来做：

```js
this.setState({
  test: [...data, ...this.state.test]
})
```

`...` 是扩展运算符，可以把数组转为用逗号分隔的参数序列。

上面那种做法每次都会新开辟数组的内存空间，如果数据量非常大并且更新很快时，容易造成内存回收不及时而导致页面死掉。所以把新来的数据 `data` 加到 `test` 这个 state 的数组的头部更好的方法是使用 `concat()`

```js
data.concat(test)
```
