---
title: "前端基础知识总结"
date: 2018-06-21T19:58:48+08:00
draft: false
slug: "Frontend-Basic-Knowledge"
---

首先，我是非常排斥靠记忆东西来应对面试，但是，有时候我们虽然在用一些东西，而面试中问到了我们没有表达出来，或者因为对概念记忆不深，就导致某次面试，记忆卡壳，导致明明自己会的东西也没回答上来，就会导致场面很尴尬，所以好记性不如烂笔头，总结下前端基础知识以备忘。

以下知识不像常见的搬运网上的文章，而是经过了自己的总结和实践。

# JavaScript

## 继承

JS 里最常用的继承方式是原型链继承，通常通过 `prototype` 关键字来实现，`prototype` 属性使你有能力向对象添加属性和方法。

```js
function people(name,job,age) {
  this.name = name;
  this.job = job;
  this.age= age;
}

var bill = new people("Bill Gates","Engineer",50);

people.prototype.salary = null;
bill.salary = 20000;
console.log(bill.salary);    // output 20000
```