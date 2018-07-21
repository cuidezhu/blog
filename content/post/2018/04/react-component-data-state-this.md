---
title: "存储React组件中的数据何时用State或者This"
date: 2018-04-10T00:59:22+08:00
draft: false
slug: "react-component-data-state-this"
---

## 为什么需要this

React组件内的数据一般用state来存储，但是如果所有的除props之外的数据都用state来存储的话，前些天做项目时就发现一个问题。

我遇到的那种场景，this.setState改变一个state的值，然后立即去fetch一个url中带有这个state参数的api，就会导致这个state的值是上一次的值，fetch到的数据是旧的，setState执行慢了一点点。本想加个setTimeout延迟几毫秒给解决掉，又一想，这样不是正经程序员所为，就把这个state改为this.x=‘123’这样的属性，这样就能解决setState异步执行带来的问题。

