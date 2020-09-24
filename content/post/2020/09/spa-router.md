---
title: "前端路由原理"
date: 2020-09-24T21:38:58+08:00
draft: false
slug: "spa-router"
---

现在我们稍微复杂一点的单页面应用（SPA），都需要路由， vue-router 也是 Vue 全家桶的标配之一。vue-router 默认 hash 模式，如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。

网页 url 组成部分：

```js
// http://127.0.0.1:8881/01-hash.html?a=100&b=20#/aaa/bbb
location.protocol  // 'http:'
location.hostname // '127.0.0.1'
location.host   // '127.0.0.1:8881'
location.port   // '8881'
location.pathname   // '/01-hash.html'
location.search   // '?a=100&b=20'
location.hash   // '#/aaa/bbb'
```

## hash 模式

hash 的特点：

* hash 变化会触发网页跳转，即浏览器的前进、后退
* hash 变化不会刷新页面，SPA 必需的特点
* hash 永远不会提交到 server 端（前端自生自灭）
  
```html
<p>hash test<p>
<button id="btn1">修改 hash</button>

<script>
  // hash 变化，包括：
  // a. JS 修改 url
  // b. 手动修改 url 的 hash
  // c. 浏览器前进、后退
  window.onhashchange = (event) => {
    console.log('old url', event.oldURL);
    console.log('new url', event.newURL);

    console.log('hash:', location.hash);
  }

  // 页面初次加载，获取hash
  document.addEventListener('DOMContentLoaded', () => {
    console.log('hash:', location.hash);
  })

  // JS 修改 url
  document.getElementById('btn1').addEventListener('click', () => {
    location.href = '#/user';
  })
</script>
```

## HTML5 history 模式

* 用 url 规范的路由，但跳转时不刷新页面
* history.pushState
* window.onpopstate

```html
<p>history API test</p>
<button id="btn1">修改 url</button>

<script>
// 页面初次加载，获取 path
document.addEventListener('DOMContentLoaded', () => {
  console.log('load', location.pathname);
})

// 打开一个新的路由
// 注意用 pathState 方式，浏览器不会刷新页面
document.getElementById('btn1').addEventListener('click', () => {
  const state = { name: 'page1' };
  console.log('切换路由到'， 'page1');
  history.pushState(state, '', 'page1');
})

// 监听浏览器前进、后退
window.onpopstate = (event) => {
  console.log('onpopstate', event.state, location.pathname);
}

</script>
```
