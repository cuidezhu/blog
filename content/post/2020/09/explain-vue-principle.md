---
title: "详解 Vue 原理"
date: 2020-09-01T06:32:40+08:00
draft: false
slug: "explain-vue-principle"
---

## 组件化和 MVVM 模式

组件化其实很久以前就有的概念，Vue 也是把这个概念借鉴了过来，允许我们使用小型、独立和通常可复用的组件构建大型应用。传统组件，只是静态渲染，更新还要依赖于操作 DOM，而 Vue 和 React 这种现代话框架则是采用数据驱动视图的方式，Vue 是采用 MVVM 模式来实现，React 是通过 setState 来实现的。所谓数据驱动视图就是我们不再直接操作 Dom，想改页面上的什么东西，直接更改 Vue 或 React 里的数据就行了，Vue 和 React 框架本身根据数据来帮我们重新渲染视图，这使得我们的开发更加关注于数据和业务逻辑，降低程序开发的复杂度，使得出现了很多富客户端应用，所谓富客户端就是页面的功能非常复杂，具有很强交互性来给客户提供更高和更全方位的体验。

Vue 的 MVVM 模式如下图所示：View 就是视图，也就是页面上的 DOM。Model 就是模型，可以理解为 Vue 组件里面的 Data，或者 Vuex 里面的数据。这两者之间通过 ViewModel 这一层来做关联，比如监听事件，通过 ViewModel 这一层，我们在 Model 修改时，就能执行页面渲染。而 View 这一层里面比如点击事件，各种 Dom 事件触发时就能修改 Model 这一层里面的数据。这就是数据驱动视图，不用直接操作视图，而是通过修改数据来操作的。MVVM 第一个 M 对应着Model，第二个字母 V 对应着 View，最后两个字母 VM 对应着 ViewModel。

![Vue MVVM](https://static.intj.top//20200901100345.png)

## Vue 响应式

### 监听 data 变化

我们知道 Vue 组件 data 的数据一旦变化，立刻触发视图的更新。首先我们要如何知道 data 是发生了变化呢？实现数据驱动视图的第一步就是当 data 发生了变化，我们怎样第一时间知道？

#### 监听对象

核心 JavaScript API：Object.defineProperty()，Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

Vue 监听对象就是利用 Object.defineProperty() 的基本用法来实现监听的，get 是属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。set 属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。：

```js
let data = {};
let name = 'zhangsan'

Object.defineProperty(data, 'name', {
  get: function() {
    console.log('get');
    return name;
  },
  set: function(newVal) {
    console.log('set');
    name = newVal;
  }
});

// 测试
console.log(data.name); // get zhangsan
data.name = 'lisi' // set
```

我们通过上面的代码可以看到访问和修改属性值的时候打印出来 `console.log` 内容，这样我们就实现了 JavaScript 对象变化时的监听。

#### 监听复杂对象

如果一个对象的属性值还是对象的话，那么需要深度监听，需要递归到底，一次性计算量大，这是一个问题（Vue 3.0 会改善这个问题）。

深度监听实现代码如下：

```js
// 触发更新视图
function updateView() {
  console.log('视图更新');
}

// 重新定义数组原型
const oldArrayProperty = Array.prototype
// 创建新对象，原型指向 oldArrayProperty, 再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
  arrProto[methodName] = function() {
    updateView();   // 触发视图更新
    oldArrayProperty[methodName].call(this, ...arguments);
  }
});

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
  // 深度监听
  observer(value);

  // 核心 API
  Object.defineProperty(target, key, {
    get() {
      return value;
    },
    set(newValue) {
      if (newValue !== value) {
        // 深度监听
        observer(value);
        // 设置新值
        // 注意，value 一直在闭包内，此处设置完之后，再 get 时也是会获取最新的值
        value = newValue;

        // 触发更新视图
        updateView();
      }
    }
  })
}

// 监听对象属性
function observer(target) {
  if (typeof target !== 'object' || target === null) {
    // 不是对象或数组
    return target;
  }

  if (Array.isArray(target)) {
    target.__proto__ = arrProto;
  }

  // 重新定义各个属性 (for in 也可以遍历数组)
  for (let key in target) {
    defineReactive(target, key, target[key]);
  }
}

// 准备数据
let data = {
  name: 'zhangsan',
  age: 20,
  info: {
    address: '北京'
  },
  nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 测试
data.name = 'lisi';
data.age = 21;
// data.age = { num: 21 }  针对 defineReactive 函数 set 时深度监听的情况
// data.age.num = 22;
data.x = '100';   // 新增属性，监听不到，因为流程不会走到 Object.defineProperty 里面 -- 所以有 Vue.set()
delete data.name;    // 删除属性，监听不到 -- 所以有 Vue.delete()
data.info.address = '上海'    // 深度监听
data.nums.push(4)   // 监听数组
```

#### Object.defineProperty() 的一些缺点

无法监听新增属性/删除属性（需要Vue.set Vue.delete）

Vue 3.0 启用 Proxy 来实现响应式，但是 Proxy 有兼容性问题，且无法 polyfill，IE 11 不支持 Proxy，所以 Vue 3.0 可能会出现一个兼容 IE 11 的版本，把 Proxy 丢掉，还原到 Object.defineProperty()。

## 虚拟 DOM (Virtual DOM) 

vdom 就是用 JS 对象模拟 DOM 结构，然后对比计算出最小的变更，再把差异应用到真正的 DOM 上。

那么如何用 JS 对象模拟 DOM 结构呢？可以看下面一个例子，比如我们有如下的 DOM 结构：

```html
<div id="div1" class="container">
  <p>vdom</p>
  <ul style="font-size: 20px">
    <li>a</li>
  </ul>
</div>
```

我们就可以这样用 JS 对象来模仿如上的 DOM 结构：

```js
{
  tag: 'div',
  props: {
    className: 'container',
    id: 'div1'
  },
  children: [
    {
      tag: 'p',
      children: 'vdom'
    },
    {
      tag: 'ul',
      props: {style: 'font-size: 20px' },
      children: [
        {
          tag: 'li',
          children: 'a'
        }
      ]
    }
  ]
}
```

## diff 算法

diff 算法即比较两颗树的不同，目前 Vue、React 的 diff 算法通过如下的一些策略，把时间复杂度优化到 O(n)：

* 只对比同一层级，不跨级比较
* tag 不相同，则直接删掉重建，不再深度比较
* tag 和 key，两者都相同，则认为是相同节点，不再深度比较

## 模版编译

* 模版编译为 render 函数，这个过程用的 JS 的 with 语法，with语句 扩展一个语句的作用域链。执行 render 函数返回 vnode
* 基于 vnode 再执行 patch（Vue 中 diff 算法过程就是调用名为patch函数） 和 diff
* 使用 webpack vue-loader，会在开发环境下编译模版

## Vue 组件渲染和更新过程

### 初次渲染过程

* 解析模版为 render 函数（或在开发环境已完成，vue-loader）
* 触发响应式，监听 data 属性 getter setter
* 执行 render 函数，生成 vnode, patch(elem, vnode)

### 更新过程

* 修改 data，触发 setter（此前在 getter 中已被监听）
* 重新执行 render 函数，生成 newVnode
* patch(vnode, newVnode) 用 newVnode 来替换老的 vnode

## Vue 常见的性能优化方式

* 合理使用 v-show 和 v-if
* 合理使用 computed
* v-for 时加 key，以及避免和 v-if 同时使用，因为同时使用时 v-for 的优先级比 v-if 高，导致每次 v-for 时都会重新计算 v-if，造成性能的浪费
* 自定义事件、DOM 事件及时销毁，避免内存泄漏
* 合理使用异步组件
* 合理使用 keep-alive
* data 层级不要太深，避免深度监听计算次数过多
* 使用 vue-loader 在开发环境做模版编译（预编译）
* 使用 SSR
