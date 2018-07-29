---
title: "对比两个 JS 对象是否相等"
date: 2018-07-28T23:42:47+08:00
draft: false
slug: "compare-two-js-object"
---

我们知道 JS 对象引用数据类型，存储的是对象在内存中的地址，故下面两个对象是不相等的：

```js
let obj = {name: 1}
let obj1 = {name: 1}

console.log(obj == obj1)  //false
```

那么我们怎么比较两个对象是否相等呢？还有如果对象内的属性值还有对象呢，我们应当递归处理这种情况。

```js
let obj = {name: 1, other: {title: 'react'}}
let obj1 = {name: 1, other: {title: 'react'}}

function compareObj(obj1, obj2) {
  if (obj1 == obj2) {
    return true
  }

  if (Object.keys(obj1).length !== Object.keys(obj2).length) {
    return false
  }
  for(let k in obj1) {
    if (typeof obj1[k] == 'object') {
      return compareObj(obj1[k], obj2[k])
    } else if (obj1[k] !== obj2[k]) {
      return false
    }
  }

  return true
}
console.log(compareObj(obj,obj1))  //true
```

但上面那种递归的复杂度较高，所以在 React 做数据对象对比时，做了一些妥协，只会做浅层对比，即不会考虑对象的属性值还是对象的情况。

```js
for(let k in obj1) {
  if (obj1[k] !== obj2[k]) {
    return false
  }
}
```