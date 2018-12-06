---
title: "解决因含有恶意代码的 flatmap-stream 包被删导致项目安装依赖错误"
date: 2018-12-06T17:23:02+08:00
draft: false
slug: "yarn-flatmap-stream"
---

最近同事在开发一个之前的 React 项目时，发现安装一个 `cookie` npm 包会报错，然后我紧接着发现那个项目无论安装什么包都会报错，把项目的 `node_modules` 目录删除后重新 `yarn install` 时也会报相同的错误：

```zsh
error http://registry.npm.taobao.org/flatmap-stream/download/flatmap-stream-0.1.1.tgz: Extracting tar content of undefined failed, the file appears to be corrupt: "Unexpected end of data"
```

看到错误相信，想到最近 `event-stream` 引入了含有恶意代码的 `flatmap-stream` 盗取用户的比特币的新闻。于是查看项目中是否有引入 `flatmap-stream` 这个包：

```zsh
yarn why flatmap-stream
```

![flatmap-stream](/img/2018/12/flatmap-stream.png)

可以看到包的依赖顺序为 `npm-run-all#ps-tree#event-stream#flatmap-stream`，最终是`npm-run-all`这个包用到了`flatmap-stream` 这个包，而这个包因某个版本含有病毒被 NPM 官方删除了。所以会导致下载时 404 错误。

注意如果你同事那里有报错，而你的电脑上安装依赖时没有报错，可能时因为你电脑上有 Yarn 的缓存，可以先清空 Yarn 的缓存：

```zsh
yarn cache clean
```

然后删除项目的 `node_modules` 目录，再重新 `yarn install` 时就会报和你同事相同的 `flatmap-stream` 错误了。

再删除 `node_modules` 之前，我看了一下 `node_modules` 下的 `flatmap-stream` 包下面内容，并没有发现恶意代码，估计之前病毒作者只在某个版本下的 index.min.js 里写入了恶意代码，使 index.js 和 index.min.js 内容不一致，而因为恶意代码在 index.min.js 里，所以不会被人轻易发现。

## 解决方法

发现新版本的 `npm-run-all` 包已经解决了 `event-stream` 包被注入恶意代码的问题。所以我们只要升级 `npm-run-all` 这个包就好了。

```zsh
yarn upgrade npm-run-all
```

然后重新 `yarn install` 就不会报错了，解决了 `flatmap-stream` 被删除导致的安装依赖报错的问题。
