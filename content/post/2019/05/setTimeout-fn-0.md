---
title: "setTimeout(fn, 0) 意味着什么"
date: 2019-05-16T23:56:25+08:00
draft: false
slug: "setTimeout-fn-0"
---

当你定义 setTimeout 那一刻起（不管时间是不是0），JS 并不会直接去执行这段代码，而是把它扔到一个事件队列里面，当页面中所有同步任务都干完了以后，才会去执行事件队列里面的代码。

