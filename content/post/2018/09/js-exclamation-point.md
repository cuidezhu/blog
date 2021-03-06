---
title: "JS 双感叹号 (!!) 作用"
date: 2018-09-18T16:21:23+08:00
draft: false
slug: "js-exclamation-point"
---

我们在 JS 代码中看到 `!!a` 这样两个感叹号代表什么呢？第一个感叹号为把变量取反转化成 boolean 类型，null、undefined 和空字符串取反之前本身可以视为 false，其余都为 true。第二个感叹号把第一个取反操作再取反，来得到变量原本应该表达的 true 或者 false。

这样写主要是为了把任意类型的数据转换成 boolean 数据类型。

比如我们在和后端 API 进行交互式，经常需要判断数据是否为空，这时就可以这样写：

```js
if (!!a) {
  // a 非空时才执行的代码
}
```

而不必写各种臃肿的判断条件了。