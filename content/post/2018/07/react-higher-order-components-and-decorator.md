---
title: "React 高阶组件和装饰器"
date: 2018-07-20T19:56:36+08:00
draft: false
slug: "react-higher-order-components-and-decorator"
---

## 高阶组件

高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。但高阶组件本身并不是React API，它只是一种模式。

下面我们来实现一个高阶组件:

```js
function WrapperHello(Comp) {
  class WrapComp extends React.Component {
    render() {
      return (
        <div>
          <p>这是 HOC 高阶组件特有的元素</p>
          <Comp {...this.props}></Comp>
        </div>
      )
    }
  }

  return WrapComp
}

class Hello extends React.Component {
  render() {
    return <h2>Hello, I love React&Redux</h2>
  }
}



Hello = WrapperHello(Hello)
```

## 装饰器写法

我们把 `Hello = WrapperHello(Hello)` 改为装饰器的写法就是下面这样：

```js
function WrapperHello(Comp) {
  class WrapComp extends React.Component {
    render() {
      return (
        <div>
          <p>这是 HOC 高阶组件特有的元素</p>
          <Comp {...this.props}></Comp>
        </div>
      )
    }
  }

  return WrapComp
}

@WrapperHello
class Hello extends React.Component {
  render() {
    return <h2>Hello, I love React&Redux</h2>
  }
}
```

上面就是一个最简单的高阶组件，我们使用高阶组件把原来的包装一层，可以把原来的组件添加或者增强一些功能。高阶组件有两种功能：属性代理和反向继承，上面的用法就是属性代理。反向继承是指返回的组件去继承之前的组件