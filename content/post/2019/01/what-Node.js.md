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

Web 场景，高并发，I/O 密集。

I/O 密集：文件操作、网络操作、数据库相关操作。

那么什么是 CPU 密集呢？如果程序大部分时间用来做压缩、解压、加密、解密这些操作就称之为 CPU 密集。

Node.js 在处理高并发，I/O 密集场景性能优势明显。

高并发简单地说就是在单位时间内访问量特别大就是高并发，高并发解决办法：

* 增加机器数
  
* 增加每台机器的 CPU 数 -- 多核

那么在公司中主要用 Node.js 做什么呢？

* Web Server
  
* 本地代码构建，如 webpack

* 实用工具开发