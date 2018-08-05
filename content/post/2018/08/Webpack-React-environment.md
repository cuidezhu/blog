---
title: "使用 Webpack 从零开始手动搭建 React 环境"
date: 2018-08-02T23:30:36+08:00
draft: false
slug: "Webpack-React-environment"
---

新建一个项目的文件夹，打开终端，进入我们新建的文件夹目录，我们使用 `npm init` 来初始化一个 npm 项目，按照提示填写项目相关信息，然后它会根据你填的信息生成一个 `package.json` 文件。

我们的项目需要用到 Webpack 和 React，这里我们用 webpack 的版本 3.9.1，我们首先安装下这两个包：

```zsh
npm i webpack@3.9.1 react
```

`npm i` 是 `npm install` 的别名，其它 `npm install` 的别名有 `isntall, add`，`npm install` 的后缀的意思可以看 [官方文档](https://docs.npmjs.com/cli/install)：

> npm install saves any specified packages into dependencies by default. Additionally, you can control where and how they get saved with some additional flags:

> -P, --save-prod: Package will appear in your dependencies. This is the default unless -D or -O are present.

> -D, --save-dev: Package will appear in your devDependencies.

> -O, --save-optional: Package will appear in your optionalDependencies.

> --no-save: Prevents saving to dependencies.

总结就是一句话，`npm install` 默认是把包添加到 package.json 文件的 `dependencies` 里，即会在开发和生产环境中都会用到，而加了 `-D` 就把包添加到 `devDependencies` 里，只会在开发中使用。至于另一个后缀 `-S` 应该是误用或者以往的用法，效果也是和默认一样，包会添加到 `dependencies` 里。

我们在项目跟目录下新建 `build` 文件夹，然后新建 `webpack.config.js` 文件，在这个文件中，我们写我们的 webpack 配置：

```js
const path = require('path')

module.exports = {
  entry: {
    app: path.join(__dirname, '../client/app.js')
  },
  output: {
    filename: '[name].[hash].js',
    path: path.join(__dirname, '../dist'),
    publicPath: '/public'
  },
  module: {
    rules: [
      {
        test: /.jsx$/,
        loader: 'babel-loader'
      }
    ]
  }
}
```

我们用 `babel-loader` 这个 loader 来让 webpack 能处理 JSX：

```zsh
npm i babel-core babel-loader -D
```

为了能支持 ES2015 和 React，我们在根目录下新建 `.babelrc` babel 的配置文件，内容如下：

```js
{
  "presets": [
    ["es2015", { "loose": true }],
    "react"
  ]
}
```

上面的配置还需要安装如下包：

```zsh
npm i babel-preset-es2015 babel-preset-es2015-loose babel-preset-react -D
```

然后我们在 `package.json` 文件 `scripts` 字段里新增如下命令：

```js
"build": "webpack --config build/webpack.config.js"
```

在终端里运行 `npm run build` 就可以编译成功编译我们的代码了，打包完成的代码会放在根目录下的 `dist` 目录。

然后我们安装 `html-webpack-plugin` 插件来把我们 Webpack 打包后的 entry 下的所有文件都注入到一个生成的 HTML 页面里面，文件名字路径都是根据 output 的配置拼接而成。

```zsh
npm i --save-dev html-webpack-plugin
```

然后在我们的 `webpack.config.js` 文件中加入以下内容：

```js
const HTMLPlugin = require('html-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    new HTMLPlugin()
  ]
}
```

这时我们再 `npm run build` 就会看到 `dist` 目录下生成了 index.html 文件，并且文件内引入了打包后的 JS 文件。

然后加入对 `.js` 结尾的文件的编译 babel-loader 配置，因为 `node_modules` 目录下的文件已经经过编译了，所以我们将这个目录排除：

```js
// ...

module.exports = {
  // ...
  module: {
      // ...
      {
        test: /.js$/,
        loader: 'babel-loader',
        exclude: [
          path.join(__dirname, '../node_modules')
        ]
      }
    ]
  },
}
```

这时我们再次  `npm run build`，打开生成的 `index.html` 文件，就会看到正确的显示结果了。