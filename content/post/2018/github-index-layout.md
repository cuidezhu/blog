---
title: "调整 Github 首页布局"
date: 2018-05-18T08:28:02+08:00
draft: false
slug: github-index-layout
---

GitHub 首页的改版好蛋疼，原先的右栏换成了左栏，左栏换成了右栏。看着好别扭，原先的看习惯了，于是想把 Github 的首页布局改回去。

首先我是用的 Chrome 浏览器，下载 [Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) ,Tampermonkey 也有其它浏览器版本，可以去 [官网](https://tampermonkey.net/) 下载你使用的浏览器对应版本。然后打开GitHub首页，点击插件图标，点添加新脚本。

在新打开的页面中，添加下面一行代码：

```
document.getElementsByClassName('column')[0].style.float="right"
```

改下 `@match`, 在`https://github.com/` 后添加 `*`, 由于 Chrome 插件应该是不支持正则表达式中的 `?`,  https://developer.chrome.com/extensions/match_patterns 可能所有的 Chrome 插件都支持个通配符 `*`, 所以想匹配 `https://github.com/orgs/xxx` 其中`xxx`代表任意字符只能加个 `*` 了，不过这样做满足了匹配首页和 orgs 页面的要求，由于其它页面并没有 `column` 这个 class,所以对其它页面无影响。

按 `Ctrl + S` 保存，然后就可以看到 Github 首页布局恢复了。

完整的代码如下:

```
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://github.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    document.getElementsByClassName('column')[0].style.float="right"
})();
```
