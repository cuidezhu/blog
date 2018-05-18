---
title: "调整Github首页布局"
date: 2018-05-18T08:28:02+08:00
draft: false
slug: github-index-layout
---

GitHub 首页的改版好蛋疼，原先的右栏换成了左栏，左栏换成了右栏。看着好别扭，原先的看习惯了，于是想把 Github 的首页布局改回去。

首先我是用的 Chrome 浏览器，下载 [Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) ,然后打开GitHub首页，点击插件图标，点添加新脚本。

在新打开的页面中，添加下面一行代码：

```
document.getElementsByClassName('column')[0].style.float="right"
```
按 `Ctrl + S` 保存，然后就可以看到 Github 首页布局恢复了。

完整的代码如下:

```
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://github.com/
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    document.getElementsByClassName('column')[0].style.float="right"
})();
```
