---
title: "JS 获取 URL 中 GET 参数的值（正则法和 location.search 法）"
date: 2018-08-03T19:43:08+08:00
draft: false
slug: "js-url-get-params"
---

## 正则法


## location.search 法

```js
function getSearch() {
  let url = location.search;
  let theRequest = new Object();

  if (url.indexOf("?") != -1) {
    let str = url.substr(1);
    strs = str.split("&");

    for(var i = 0; i < strs.length; i ++) {
      theRequest[strs[i].split("=")[0]] = unescape(strs[i].split("=")[1]);
    }
  }

  return theRequest;
}
```