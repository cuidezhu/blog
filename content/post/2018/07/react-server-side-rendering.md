---
title: "React 服务端渲染（SSR）"
date: 2018-07-24T21:38:00+08:00
draft: false
slug: "react-server-side-rendering"
---

服务端渲染有利于首屏加载速度和搜索引擎优化（SEO）。

## babel-node

因为 Node 环境原生不支持 JSX，首先我们使用 `babel-node` 让 Node 环境支持 JSX：

```js
yarn add babel-cli
```

然后我们在 `package.json` 里用 `babel-node` 来替代 `Node`，"scripts" 字段里加入下面一行：

```js
"scripts": {
  "server": "NODE_ENV=test nodemon --exec babel-node server/server.js",
}
```

现在我们的 Node 环境已经支持 es6 了，那么怎么支持 JSX 呢？我们在项目根目录下新建个 babel 的配置文件 `.babelrc`，然后把我们的 `package.json` 文件下的 babel 的配置复制到 `.babelrc` 文件中：

```js
{
  "presets": [
    "react-app"
  ],
  "plugins": [
    "transform-decorators-legacy",
    [
      "import",
      {
        "libraryName": "antd-mobile",
        "style": "css"
      }
    ]
  ]
}
```

## renderToString()

然后我们把 src/index.js 下的内容复制到服务端 server/server.js中

```js
let context = {}
const markup = renderToString(
  (<Provider store={store}>
    <StaticRouter
      location={req.url}
      context={context}
    >
      <App></App>
    </StaticRouter>
  </Provider>)
)
```

## css-modules-require-hook 解决 CSS 报错

Node 里是没有 CSS 的，所以我们用 css-modules-require-hook 这个包来解决 CSS 报错相关问题：

```js
yarn add css-modules-require-hook
```

在服务端的代码中 Route 相关代码之前引入 csshook：

```js
import csshook from 'css-modules-require-hook/preset' // import hook before routes
```

然后在项目根目录下建立文件 cmrh.conf.js：

```js
// cmrh.conf.js
module.exports = {
  // Same scope name as in webpack build
  generateScopedName: '[name]__[local]___[hash:base64:5]',
}
```

## asset-require-hook 解决图片报错

在 servder.js 代码中加入下列代码：

```js
import assethook from 'asset-require-hook'
assethook({
  extensions: ['png']
})
```

## 拼接 public/index.html 骨架

把服务端生成的 html 放到 html 骨架里：

```js
const pageHtml = `<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="theme-color" content="#000000">

    <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">

    <title>React App</title>
  </head>
  <body>
    <noscript>
      You need to enable JavaScript to run this app.
    </noscript>
    <div id="root">${markup}</div>
  </body>
</html>`

res.send(pageHtml)
```

现在我们的 html 页面就是带有骨架的完整页面了。

## 引入 CSS 和 JS 文件

我们 build 项目生成的 build 文件夹下的 `build/asset-manifest.json` 文件中有 CSS 和 JS 的路径，因为文件名每次 build 每次都在变，这样做是为了在文件名中加入 hash 值使之不命中缓存。

```js
import staticPath from '../build/asset-manifest.json'
```

然后我们在 html 骨架中加入下面两行代码来分别引入 CSS 和 JS

```js
<link rel="stylesheet" href="/${staticPath['main.css']}">

<script src="/${staticPath['main.js']}"></script>
```

重启项目我们发现我们服务端渲染的首页已经正常显示了。

## React 16 renderToNodeStream() 新 API 实现服务端渲染

直接渲染成 Node 节点流，通过这个节点流给浏览器流式地返回，我们把我们上面的 renderToString() 改为 React 16 的renderToNodeStream()

```js
const objDes = {
  '/msg': 'React Chat',
  '/boss': 'boss genius'
}

res.write(`<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="theme-color" content="#000000">
    <meta name="keywords" content="React, Redux, SSR, ${objDes[req.url]}">

    <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
    <link rel="stylesheet" href="/${staticPath['main.css']}">

    <title>React App</title>
  </head>
  <body>
    <noscript>
      You need to enable JavaScript to run this app.
    </noscript>
    <div id="root">`)
const markupStream = renderToNodeStream(
  (<Provider store={store}>
    <StaticRouter
      location={req.url}
      context={context}
    >
      <App></App>
    </StaticRouter>
  </Provider>)
)

markupStream.pipe(res, {end: false})
markupStream.on('end', ()=> {
  res.write(`</div>
  <script src="/${staticPath['main.js']}"></script>
  </body>
</html>`)
  res.end()
})

```

然后把 `src/index.js` 里的 `ReactDOM.render()` 改为 `ReactDOM.hydrate()`

然后我们就完成流式渲染了。