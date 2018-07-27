---
title: "JavaScript 函数柯里化（Curry）"
date: 2018-07-27T18:41:39+08:00
draft: false
slug: "JavaScript-function-curry"
---


```js
const add = function(x) {
  return function(y) {
    return x + y + 3
  }
}
const add = x => y => x+y+3
const res = add(2)(4)

console.log('res is', res)
```