---
title: "React 高阶组件和装饰器"
date: 2018-07-20T19:56:36+08:00
draft: false
slug: "react-higher-order-components-and-decorator"
---

## 高阶组件

高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。但高阶组件本身并不是React API，它只是一种模式。

下面我们来实现一个高阶组件。