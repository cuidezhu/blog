---
title: "Apply, Call 和 Bind"
date: 2018-07-04T23:58:57+08:00
draft: false
slug: "Apply, Call 和 Bind"
---

在 JavaScript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。

```js
let obj = {
  name: 'jack'
}

function func() {
  console.log(this.name);
}

func.call(obj);       // "jack"
```

call 方法的第一个参数是作为函数上下文的对象，这里把 obj 作为参数传给了 func，此时函数里的 this 便指向了 obj 对象。此处 func 函数里其实相当于

```js
function func() {
  console.log(obj.name);
}
```

call 和 apply 作用相同，只是接收的参数不一样

```js
function.apply(thisArg, [argsArray])
function.call(thisArg, arg1, arg2, ...)
```

bind() 也可以改变 this 的指向，bind() 方法会创建一个新函数，当这个新函数被调用时，它的 this 值是传递给 bind() 的第一个参数, 它的参数是 bind() 的其他参数和其原本的参数。

apply, call 和 bind 三者有相似之处，

1. 都是用来改变函数的this对象的指向的。
2. 第一个参数都是this要指向的对象。
3. 都可以利用后续参数传参。

```js
var xw={
  name: "小王",
  gender: "男",
  age: 24,
  say: function() {
    alert(this.name+" , "+this.gender+" ,今年"+this.age);
  }
}
var xh={
  name: "小红",
  gender: "女",
  age: 18
}
xw.say();
```

输出的是： `小王 , 男 ,今年24`。

那么我们如何输出小红的数据呢？

用 `apply` 的话是这样：

```js
xw.say.apply(xh);
```

用 call 的话也类似：

```js
xw.say.call(xh)
```

用 bind 的话是这样：

```js
xw.say.bind(xh)();
```

call和apply都是对函数的直接调用，而bind方法返回的仍然是一个函数，因此后面还需要()来进行调用才可以。

另一个区别就是 bind 的传参数可以在 `bind()` 里传参数：

```js
xw.say.bind(xh,"实验小学","六年级")();
```

也可以在调用时传参数：

```js
xw.say.bind(xh)("实验小学","六年级");
```
