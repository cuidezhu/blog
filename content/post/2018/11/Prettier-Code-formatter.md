---
title: "在 VS Code 中使用 Prettier 来格式化前端代码"
date: 2018-11-16T19:18:34+08:00
draft: false
slug: "Prettier-Code-formatter"
---

我们平时写代码经常需要保持一定的风格，而 [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 可以很好的帮我们格式化 JavaScript / TypeScript / CSS，Prettier 甚至支持格式化 SCSS 代码，而我们如果使用官方的 sass-convert 往往会遇到这个工具自身的兼容性问题。

首先我们在 VS Code 里安装 [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)， 然后我们进行一些个性化配置，打开 VS Code 的配置文件 settings.json，然后在其中加入下面内容：

```json
  "editor.formatOnSave": true,
  "prettier.jsxSingleQuote": true,
  "prettier.semi": false
```

然后每次保存的时候就会自动格式化代码了，你也可以通过 `CMD + Shift + P -> Format Document` 来手动进行格式化代码。
