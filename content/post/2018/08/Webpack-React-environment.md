---
title: "使用 Webpack 从零开始手动搭建 React 环境"
date: 2018-08-02T23:30:36+08:00
draft: false
slug: "Webpack-React-environment"
---

新建一个项目的文件夹，打开终端，进入我们新建的文件夹目录，我们使用 `npm init` 来初始化一个 npm 项目，按照提示填写项目相关信息，然后它会根据你填的信息生成一个 `package.json` 文件。

我们的项目需要用到 Webpack 和 React，我们首先安装下这两个包：

```zsh
npm i webpack react
```

`npm i` 是 `npm install` 的别名，其它 `npm install` 的别名有 `isntall, add`，`npm install` 的后缀的意思可以看 [官方文档](https://docs.npmjs.com/cli/install)：

> npm install saves any specified packages into dependencies by default. Additionally, you can control where and how they get saved with some additional flags:

> -P, --save-prod: Package will appear in your dependencies. This is the default unless -D or -O are present.

> -D, --save-dev: Package will appear in your devDependencies.

> -O, --save-optional: Package will appear in your optionalDependencies.

> --no-save: Prevents saving to dependencies.

总结就是一句话，`npm install` 默认是把包添加到 package.json 文件的 `dependencies` 里，即会在开发和生产环境中都会用到，而加了 `-D` 就把包添加到 `devDependencies` 里，只会在开发中使用。至于另一个后缀 `-S` 应该是误用或者以往的用法，效果也是和默认一样，包会添加到 `dependencies` 里。