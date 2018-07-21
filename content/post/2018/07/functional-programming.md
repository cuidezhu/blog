---
title: "函数式编程"
date: 2018-07-21T12:09:18+08:00
draft: false
slug: "functional-programming"
---

函数式编程是一种编程典范，最重要的是函数可以当参数，函数也可以当返回值。比如下面这个例子：

```js
function hello() {
  console.log('hello, I love react')
}


function WrapperHello(fn) {
  return function() {
    console.log('before say hello')
    fn()
    console.log('after say hello')
  }
}

hello = WrapperHello(hello)
hello()
```

上面这句 `hello = WrapperHello(hello)` 把 hello() 这个函数装饰了一次，这种设计模式称为装饰器模式，这也是 React 中的高阶组件（HOC）实现的基础。
