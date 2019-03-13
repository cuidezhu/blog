---
title: "Node.js 概念及优势"
date: 2019-01-03T23:58:26+08:00
draft: false
slug: "what-Node.js"
---

Node.js 是构建在 Chrome's V8 JavaScript 引擎上的运行时，Node.js 并不是一门语言，JavaScript 才是语言，而  Node.js 是一个 JavaScript runtime，类似于 Java 语言的 JRE，是一个运行时。

* 阻塞：I/O 时进程休眠等待 I/O 完成后进行下一步。

* 非阻塞 I/O：I/O 时函数立即返回，进程不等待 I/O 完成。

事件驱动：

* I/O 等异步操作结束后的通知，发射一个事件，然后调用相应的事件处理程序。

* 内部实现是用观察者模式实现的。

使用场景：

I/O 密集：文件操作、网络操作、数据库

Node.js 在处理高并发，I/O 密集场景性能优势明显。

高并发简单地说就是在单位时间内访问量特别大就是高并发，高并发解决办法：