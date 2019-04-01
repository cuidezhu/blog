---
title: "Node.js 所用的模块规范 -- CommonJS 规范"
date: 2019-04-01T22:43:42+08:00
draft: false
slug: "Node.js-Modules-CommonJS"
---

我们先来看一个例子，新建一个 `01_run.js` 文件：

```js
console.log("This is a test.");
```

在终端中打开该文件所在目录，执行调试 `node --inspect-brk 01_run.js`，然后在 Chrome 浏览器打开地址 `chrome://inspect/`，就可以看到我们的文件的 Target，点击 `inspect`，在打开的调试工具窗口中就可以看到实际执行的代码为：

```js
(function(exports, require, module, __filename, __dirname) {
  console.log("This is a test.");
});
```

外面包裹的函数都是 Node.js 内部帮我们做的，`exports` 参数代表模块的输出， `require` 表示需要引用别的模块时调用的 function, `module` 代表模块本身,`module` 里面还有一个`exports` 属性， `__filename` 就是该文件的路径（绝对路径）, `__dirname` 是该文件所在文件夹的路径（绝对路径）。

`exports` 参数是 `module.exports` 的简写，当 `exports` 在文件中被当作变量赋值时，则该简写就不成立了，参考 [exports shortcut](https://nodejs.org/docs/latest/api/modules.html#modules_exports_shortcut)。

```js
module.exports.hello = true; // Exported from require of module
exports = { hello: false };  // Not exported, only available in the module
```

CommonJS 规范：

* 每个文件是一个模块，一个文件内也只能有一个模块，有自己的作用域

* 在模块内部 `module` 变量代表模块本身

* `module.exports` 属性代表模块对外接口