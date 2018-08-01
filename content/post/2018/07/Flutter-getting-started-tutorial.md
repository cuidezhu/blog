---
title: "Flutter 入门教程"
date: 2018-07-31T23:43:37+08:00
draft: false
slug: "Flutter-getting-started-tutorial"
---

Flutter 是谷歌推出的一款开发移动端 App 的框架，支持 Android，iOS，和谷歌未来的新系统 Fuchsia。Flutter 解决了 React Native 的性能问题，现在学习 Flutter 正当其时，用一套代码即可支持多平台应用，为我们带来了开发效率的极大提升。

Flutter 是用 Dart 语言写的，坊间相传 Dart 开发组就在 Flutter 团队旁边，所以 Flutter 团队为了方便才用 Dart 的，其实主要是因为 Dart 语言的高性能，Dart 支持即时编译（just-in-time compilation）和预先编译（ahead-of-time Compilation），即时编译使 Flutter 能在 App 运行时直接重新编译，热加载（Hot Reloading）带来了开发效率的提高；预先编译意味着你的 App 使用的库和函数等直接编译为原生的 ARM 代码，这时发布版的 App 运行效率更高和可预测，  所以 Dart 非常适合移动开发。而且 Dart 有静态类型，随着项目规模的增大，可以帮助你更好的追踪 Bug 和管理代码。