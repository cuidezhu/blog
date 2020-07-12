---
title: "React 原理深入解析（附源码）"
date: 2018-07-15T09:51:10+08:00
draft: false
slug: "React-Source-Code-Analysis"
---

## JSX

我们现在一般使用 JSX 来写 React 代码，实际上我们的 JSX 最终会把编译成普通的 JavaScript 代码，我们可以在 [Babel](https://babeljs.io/repl) 里看下转化后的代码：

![babel-jsx](https://static.intj.top/20190214150708.png)

我们可以看到经过 Babel 编译后，React 实际上是调用 `createElement()` 函数 来创建元素的。

## React.createElement()

我们可以看下 React 源码中的 `react/packages/react/src/ReactElement.js` 文件，找到`createElement()` 函数，createElement() 接收三个参数，分别是元素的类型，属性，和子元素：

```js
/**
 * Create and return a new ReactElement of the given type.
 * See https://reactjs.org/docs/react-api.html#createelement
 */
export function createElement(type, config, children) {
  let propName

  // Reserved names are extracted
  const props = {}

  let key = null
  let ref = null
  let self = null
  let source = null

  if (config != null) {
    if (hasValidRef(config)) {
      ref = config.ref
    }
    if (hasValidKey(config)) {
      key = "" + config.key
    }

    self = config.__self === undefined ? null : config.__self
    source = config.__source === undefined ? null : config.__source
    // Remaining properties are added to a new props object
    for (propName in config) {
      if (
        hasOwnProperty.call(config, propName) &&
        !RESERVED_PROPS.hasOwnProperty(propName)
      ) {
        props[propName] = config[propName]
      }
    }
  }

  // Children can be more than one argument, and those are transferred onto
  // the newly allocated props object.
  // 这里根据 arguments.length 来判断子元素的个数，因为前两个参数分别是 type, config，所以减去2
  const childrenLength = arguments.length - 2
  if (childrenLength === 1) {
    props.children = children
  } else if (childrenLength > 1) {
    const childArray = Array(childrenLength)
    for (let i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2]
    }
    if (__DEV__) {
      if (Object.freeze) {
        Object.freeze(childArray)
      }
    }
    props.children = childArray
  }

  // 省略代码...

  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props
  )
}
```

我们可以看到最终 `createElement()` 函数最终 返回的是 `ReactElement()` 函数

## ReactElement()

目录：`react/packages/react/src/ReactElement.js`

```js
const ReactElement = function(type, key, ref, self, source, owner, props) {
  const element = {
    // This tag allows us to uniquely identify this as a React Element
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    type: type,
    key: key,
    ref: ref,
    props: props,

    // Record the component responsible for creating this element.
    _owner: owner
  }

  if (__DEV__) {
    // The validation flag is currently mutative. We put it on
    // an external backing store so that we can freeze the whole object.
    // This can be replaced with a WeakMap once they are implemented in
    // commonly used development environments.
    element._store = {}

    // To make comparing ReactElements easier for testing purposes, we make
    // the validation flag non-enumerable (where possible, which should
    // include every environment we run tests in), so the test framework
    // ignores it.
    Object.defineProperty(element._store, "validated", {
      configurable: false,
      enumerable: false,
      writable: true,
      value: false
    })
    // self and source are DEV only properties.
    Object.defineProperty(element, "_self", {
      configurable: false,
      enumerable: false,
      writable: false,
      value: self
    })
    // Two elements created in two different places should be considered
    // equal for testing purposes and therefore we hide it from enumeration.
    Object.defineProperty(element, "_source", {
      configurable: false,
      enumerable: false,
      writable: false,
      value: source
    })
    if (Object.freeze) {
      Object.freeze(element.props)
      Object.freeze(element)
    }
  }

  return element
}
```

可以看到 ReactElement() 函数最终返回的是个对象：

```js
const element = {
  // This tag allows us to uniquely identify this as a React Element
  $$typeof: REACT_ELEMENT_TYPE,

  // Built-in properties that belong on the element
  type: type,
  key: key,
  ref: ref,
  props: props,

  // Record the component responsible for creating this element.
  _owner: owner
}
```

可见 `createElement()` 函数最终创建的元素在 JavaScript 内部的形式就是上面形式的对象。

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

**React 就是用上面的 JavaScript 对象来模拟 DOM 树，称为虚拟 DOM。**

## 虚拟 DOM

当我们 `setState` 时，会生成一棵新的 DOM 树，React 会使用 Diff 算法来逐层判断新生成的树和上次树的差别，发现差异后，把差异记录下来，然后去操作真实 DOM，来达到最少操作 DOM 的目的。

React 只会对同一个层级的元素进行对比，即同一个父节点下的所有子节点。当发现节点已经不存在，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。这样只需要对树进行一次遍历，便能完成整个 DOM 树的比较。

![react-dom-diff](https://static.intj.top/20190214151006.png)

React 的 diff 算法比较复杂，在源码的 `react/packages/react-reconciler/` 目录下可以看到整套调和的源码。

我们也可以使用 `shouldComponentUpdate()` 来自己定制更新 DOM 操作。

## 把差异应用到真正的 DOM 树上

上一步我们记录下了差异，然后 React 会把差异应用到真实的 DOM 树上:

目录：`react/packages/react-dom/src/client/ReactDOMFiberComponent.js`

```js
// Apply the diff.
export function updateProperties(
  domElement: Element,
  updatePayload: Array<any>,
  tag: string,
  lastRawProps: Object,
  nextRawProps: Object
): void {
  // Update checked *before* name.
  // In the middle of an update, it is possible to have multiple checked.
  // When a checked radio tries to change name, browser makes another radio's checked false.
  if (
    tag === "input" &&
    nextRawProps.type === "radio" &&
    nextRawProps.name != null
  ) {
    ReactDOMFiberInput.updateChecked(domElement, nextRawProps)
  }

  const wasCustomComponentTag = isCustomComponent(tag, lastRawProps)
  const isCustomComponentTag = isCustomComponent(tag, nextRawProps)
  // Apply the diff.
  updateDOMProperties(
    domElement,
    updatePayload,
    wasCustomComponentTag,
    isCustomComponentTag
  )

  // TODO: Ensure that an update gets scheduled if any of the special props
  // changed.
  switch (tag) {
    case "input":
      // Update the wrapper around inputs *after* updating props. This has to
      // happen after `updateDOMProperties`. Otherwise HTML5 input validations
      // raise warnings and prevent the new value from being assigned.
      ReactDOMFiberInput.updateWrapper(domElement, nextRawProps)
      break
    case "textarea":
      ReactDOMFiberTextarea.updateWrapper(domElement, nextRawProps)
      break
    case "select":
      // <select> value update needs to occur after <option> children
      // reconciliation
      ReactDOMFiberSelect.postUpdateWrapper(domElement, nextRawProps)
      break
  }
}
```

这里会对 input 等输入表单做些检查等特别处理，上面函数调用的是 updateDOMProperties() 来更新 DOM:

目录：`react/packages/react-dom/src/client/ReactDOMFiberComponent.js`

```js
function updateDOMProperties(
  domElement: Element,
  updatePayload: Array<any>,
  wasCustomComponentTag: boolean,
  isCustomComponentTag: boolean
): void {
  // TODO: Handle wasCustomComponentTag
  for (let i = 0; i < updatePayload.length; i += 2) {
    const propKey = updatePayload[i]
    const propValue = updatePayload[i + 1]
    if (propKey === STYLE) {
      CSSPropertyOperations.setValueForStyles(
        domElement,
        propValue,
        getStackInDev
      )
    } else if (propKey === DANGEROUSLY_SET_INNER_HTML) {
      setInnerHTML(domElement, propValue)
    } else if (propKey === CHILDREN) {
      setTextContent(domElement, propValue)
    } else {
      DOMPropertyOperations.setValueForProperty(
        domElement,
        propKey,
        propValue,
        isCustomComponentTag
      )
    }
  }
}
```

上面函数使用了个 for 循环来遍历需要更新的元素，然后根据 propKey 来执行不同的操作。

至此，React 大致原理差不多就完了。
