---
title: "为 create-react-app 项目添加 Sass 支持"
date: 2018-09-02T23:34:25+08:00
draft: false
slug: "create-react-app-sass"
---

首先安装 `node-sass-chokidar`：

```zsh
yarn add node-sass-chokidar
```

然后在 `package.json` 中加入下面两条执行脚本：

```zsh
"build-css": "node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/",
"watch-css": "npm run build-css && node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",
```

`watch-css` 会监控 `src` 目录下的所有的 `.scss` 文件，然后会把每一个 `.scss` 文件生成一个对应的 `.css` 文件，我们可以在组件中 `import` 对应的 `.css` 文件。另外我们需要在 `.gitignore` 文件中忽略 `src` 目录及其子目录下的 `.css` 文件 `src/**/*.css`。

安装 `npm-run-all` 来同时执行 `npm start` 和 `watch-css`：

```zsh
yarn add npm-run-all
```

最后在 `package.json` 中删掉 `start` 和 `build` 这两条脚本，加入下面四条脚本（我的项目已 `yarn eject`）：

```zsh
"start": "npm-run-all -p watch-css start-js",
"start-js": "node scripts/start.js",
"build": "npm-run-all build-css build-js",
"build-js": "node scripts/build.js",
```

`npm-run-all -p`, 的 `-p` 参数表示并行执行任务 --parallel <tasks> - Run a group of tasks in parallel. e.g. `npm-run-all -p foo bar` is similar to `npm run foo & npm run bar`.

如果未执行过 `yarn eject`，则加入下面四条脚本：

```zsh
"start-js": "react-scripts start",
"start": "npm-run-all -p watch-css start-js",
"build-js": "react-scripts build",
"build": "npm-run-all build-css build-js",
```

至此，就完成了 create-react-app 项目对 Sass 的支持。