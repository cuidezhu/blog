---
title: "CSS Modules 及其在 React 中实践"
date: 2018-10-18T23:59:00+08:00
draft: false
slug: "CSS-Modules"
---

## CSS 样式问题

在样式开发过程中，有两个问题比较突出：

1. 全局污染 —— CSS 文件中的选择器是全局生效的，不同文件中的同名选择器，根据 build 后生成文件中的先后顺序，后面的样式会将前面的覆盖；

2. 选择器复杂 —— 为了避免上面的问题，我们在编写样式的时候不得不小心翼翼，类名里会带上限制范围的标识，变得越来越长，多人开发时还很容易导致命名风格混乱，一个元素上使用的选择器个数也可能越来越多。

## Create React App 支持 CSS Modules

为了让我们使用 Create React App 创建的项目支持 CSS Modules，我们需要把 CSS 文件的命名中加上 module 类似 [name].module.css，然后实际编译后的类名会被加上一个 hash 值，变成了这样的格式 `[filename]\_[classname]\_\_[hash]`，这保证了它的唯一性。

如果你使用了 SCSS 或者别的预处理器，也需要为相应的预处理器扩展名前面加上 module，类似于 `[name].module.scss` or `[name].module.sass`。

比如我们现在新建一个 `Button.module.css` 文件：

```css
.error {
  background-color: red;
}
```

然后在组件文件里按照如下方式使用我们定义的样式：

```js
import React, { Component } from 'react';
import styles from './Button.module.css'; // Import css modules stylesheet as styles

class Button extends Component {
  render() {
    // reference as a js object
    return <button className={styles.error}>Error Button</button>;
  }
}
```

最终组件被编译为：

```js
<button class="Button_error_ax7yz"></div>
```

样式文件名也会被编译：

```css
.Button_error_ax7yz {
  background-color: red;
}
```

## 全局作用域

如果我们想要一个全局生效的样式呢？可以使用 `:global`，CSS Modules 允许使用 `:global(.className)` 的语法，声明一个全局规则。凡是这样声明的 class，类名都不会被编译加上文件名和哈希字符串。

```css
/* 定义全局样式 */
:global(.text) {
  font-size: 16px;
}
```

借助于像 SCSS 这样的 CSS 预处理器，我们可以轻松地定义多个全局样式：

```scss
:global {
  .footer {
    color: #ccc;
  }
  .sider {
    background: #ebebeb;
  }
}
```

默认就是相当于给每一个类名添加了 `:local`，以此来实现样式的局部化：

```css
.normal {
  color: green;
}

/* 以上与下面等价 */
:local(.normal) {
  color: green;
}
```

## 组合（compose）样式

```css
.baseclassName {
  color: green;
  background: red;
}

.otherClassName {
  composes: className;
  color: yellow;
}
```

这样 `.otherClassName` 这个 class 就会包含 `.baseclassName` 这个 class 的所有样式。

## CSS 命名

我们 `import styles from './App.module.css';` 时，`styles` 的值是一个对象，我们一般通过 `.` 操作符来拿到对象属性的值：

```js
{App: "App_App__3mFxp", App-logo-spin: "App_App-logo-spin__1MXin", App-header: "App_App-header__2SAu8", App-link: "App_App-link__3bOgN"}
```

所以像上面那样的 class 名 App-logo-spin 在 JavaScript 中包含非法的标识符 `-`，所以我们命名 class 名时需要使用小驼峰命名法，类似于 `appLogoSpin`，如果是非法的标识符，我们取属性值的时候可以采用 obj[“propertyName”] 的形式，在这个例子中就应该是 `className={styles["App-header"]}`, 不过我们一般统一为 `.` 来取属性值。