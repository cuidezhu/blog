---
title: "JavaScript 函数防抖 Debounce 实现原理"
date: 2018-07-02T23:20:48+08:00
draft: false
slug: "JavaScript-Function-Debounce"
---
 
考虑这样一个场景，我们有一个搜索框，需要在用户输入内容时列出搜索结果，我们可以用 `oninput` 事件来做这件事情。

`oninput` 和 `onchange` 不同之处在于 `oninput` 事件在 input 值发生变化是立即触发， `onchange` 在元素失去焦点时触发。

我们想要选用 oninput 来做这件事情，以便即时地获取到搜索的数据。

```html
<input type="text" id="myInput" oninput="debounceSearch(this)">
```

可是用 `oninput` 会带来一个问题，比如用户想搜索 `JavaScript` 这个关键字，当用户输入 `J` 时发送一次请求数据的请求，接着输入 `a`，又用 `Ja`这个关键字发送一次请求，在用户输入完 `JavaScript` 这个关键字的过程中，我们总共发了 10 次请求，而前 9 次请求都是无效的，如果用户输入够快的话，这样就会带给服务器更大的压力，我们可以用函数防抖来解决这个问题。

函数防抖（debounce）就是当我们发送一个请求方法时，我们把请求数据的方法放到 `setTimeout()` 里，每次发请求之前都先 `clearTimeout()`，这样用户在 `setTimeout()` 里规定的 delay 时间里触发 `oninput` 就会 `clearTimeout()`，导致不发送请求，直至用户在 delay 时间里无操作，才发送请求。代码简化如下：

```js
let timer;

function debounceSearch(e) {
  let searchKeywords = e.value;

  clearTimeout(timer);

  timer = setTimeout(() => {
    console.log(searchKeywords);
  }, 250);
}
```

上面那种方法只是简单的演示下防抖的原理，上面那种方法扩展性较差

用闭包实现如下： 

```js
function debounce(fn, delay) {
  let timer;
  return function () {
    var context = this;
    var args = arguments;

    clearTimeout(timer);

    timer = setTimeout(function () {
      fn.apply(context, args);
    }, delay);
  }
}
```

```html
<input type="text" id="myInput">
```

调用方法类似如下：

```js
let el = document.getElementById("myInput");
myInput.addEventListener("input", debounce(function() {
  console.log(this.value)
  // request method code put here
  }, 250));
```

### 关于 arguments

JavaScript 的每个函数都会有一个 Arguments 对象实例 arguments，它引用着函数的实参，arguments类似于数组， 它也有类似于数组的length属性。

```js
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

用 arguments 对象判断传递给函数的参数个数，即可模拟函数重载：

```js
function doAdd() {
  if(arguments.length == 1) {
    alert(arguments[0] + 5);
  } else if(arguments.length == 2) {
    alert(arguments[0] + arguments[1]);
  }
}

doAdd(10);  //输出 "15"
doAdd(40, 20);  //输出 "60"
```

### 关于 apply()

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
