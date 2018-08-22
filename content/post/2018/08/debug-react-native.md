---
title: "调试 React Native"
date: 2018-08-21T15:32:42+08:00
draft: false
slug: "debug-react-native"
---

我们写 JavaScript 通常通过 `console.log` 来打印一些信息调试我们的程序，那么使用 React Native 开发移动 App 应该如何调试呢？

如果我们是在 Xcode 中启动的模拟器，那么我们写的 `console.log('test')` 实际上是被打印到 Xcode 的 Debug 日志里，直接在 Xcode 里就能看到。

如果我们是通过 `react-native run-ios` 来启动模拟器，在程序中写 `console.log('test')`，我们要怎么看到打印的信息呢？

我们可以在模拟器界面，按 `Command + D`，点击 `Debug JS Remotely`，就会在浏览器里打开一个新的调试页面，在这个调试页面的 Console 里可以看到我们打印的信息。

或者可以使用 `console.warn()` 直接在 App 内打印调试信息。