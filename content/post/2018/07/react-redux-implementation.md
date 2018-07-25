---
title: "实现 react-redux"
date: 2018-07-18T20:45:52+08:00
draft: false
slug: "react-redux-implementation"
---

前面的文章 [迷你 Redux 实现](ijs.me/2018/07/14/mini-redux-implemention/) 我们实现了一个简单的 Redux, 那么我们怎么在 React 项目里使用我们自制的 Redux 呢？我们可以显式传递 Store，但更优雅的方式是使用 react-redux，这篇文章会实现我们自己的 react-redux。

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

我们可以在 App 组件中将 Store 作为属性传递给它的子组件，现在可以在子组件中使用它了。

但这种方法需要属性在每层组件树中传递，就像上面的实例，我们需要把 Store 首先作为属性传递给 App 组件，App 组件再把 Store 作为属性传递给它的子组件，最后才在 App 的自组件内使用传递来的属性。如何克服这种在每一层级都传递属性的缺点呢？再看下面这个例子：

```js
import React from 'react'

class Sidebar extends React.Component {
  render() {
    return (
      <div>
        <p>侧边栏</p>
        <Navbar user={this.props.user}></Navbar>
      </div>
    )
  }
}

class Navbar extends React.Component {
  render() {
    return (
      <div>{this.props.user}的导航栏</div>
    )
  }
}

class Page extends React.Component {
  render() {
    const user = 'Jack'
    return (
      <div>
        <p>我是{user}</p>
        <Sidebar user={user}></Sidebar>
      </div>
    )
  }
}

export default Page
```

上面这种层层通过属性传递比较繁琐，而且 `Sidebar` 这个组件本身不需要 `user` 这个数据，却依然需要接收和传递数据，这对性能也是一种浪费。

React 中的 Context 就是为了解决这个问题，Context 通过组件树提供了一个传递数据的方法，从而避免了在每一个层级手动的传递 props 属性。

## Context

Context 是全局的，组件里声明，所有的子元素可以直接获取。

把上面的例子用 Context 改造如下：

```js
import React from 'react'
import PropTypes from 'prop-types'

class Sidebar extends React.Component {
  render() {
    return (
      <div>
        <p>侧边栏</p>
        <Navbar></Navbar>
      </div>
    )
  }
}

class Navbar extends React.Component {
  static contextTypes = {
    user: PropTypes.string
  }
  render() {
    console.log(this.context)
    return (
      <div>{this.context.user}的导航栏</div>
    )
  }
}

class Page extends React.Component {
  static childContextTypes = {
    user: PropTypes.string
  }
  constructor(props) {
    super(props)
    this.state = {user: 'Jack'}
  }
  getChildContext() {
    return this.state
  }
  render() {
    return (
      <div>
        <p>我是{this.state.user}</p>
        <Sidebar></Sidebar>
      </div>
    )
  }
}

export default Page
```

## 实现 react-redux

```js
import React from 'react'
import PropTypes from 'prop-types'
// connect负责链接组件，給到redux里的数据放到组件的属性里
// 1. 负责接受一个组件，把state里的一些数据放进去，返回一个组件
// 2. 数据变化的时候，能够通知组件

// function 写法写 connect
// export function connect(mapStateToProps, mapDispatchToProps) {
//   return function(WrapComponent) {
//     return class ConnectComponent extends React.Component {

//     }
//   }
// }

// 双层箭头函数的写法
export const connect = (mapStateToProps=state=>state, mapDispatchToProps={}) => (WrapComponent) => {
  return class ConnectComponent extends React.Component {
      static contextTypes = {
        store: PropTypes.object
      }

      constructor(props, context) {
        super(props)
        this.state = {
          props: {}
        }
      }

      componentDidMount() {
        this.update()
      }

      update() {
        // 获取 mapStateToProps 和 mapDispatchToProps 放入 this.props里
        const {store} = this.context.store
        const stateProps = mapStateToProps(store.getState())
      }

      render() {
        return <WrapComponent {...this.state.props}></WrapComponent>
      }
  }
}

// Provider, 把store放到context里，所有的子元素可以直接取到store

export class Provider extends React.Component {
  static childContextTypes = {
    store: PropTypes.object
  }
  
  // 把store存到context里，让子组件可取
  getChildContext() {
    return {store: this.store}
  }

  constructor(props, context) {
    super(props, context)

    // store 通过 Provider 组件的属性传进来
    this.store = props.store
  }

  render() {
    return this.props.children
  }
}
```

我们可以看到官方的 react-redux 的 Provider 也是类似的实现方法 [react-redux/src/components/Provider.js](https://github.com/reduxjs/react-redux/blob/master/src/components/Provider.js)。