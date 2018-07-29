---
title: "React 服务端渲染（SSR）"
date: 2018-07-24T21:38:00+08:00
draft: false
slug: "react-server-side-rendering"
---

服务端渲染有利于首屏加载速度和搜索引擎优化（SEO）。

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
