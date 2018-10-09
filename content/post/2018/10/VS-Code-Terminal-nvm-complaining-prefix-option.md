---
title: "解决 VS Code 终端启动时 nvm 提示 prefix option 错误"
date: 2018-10-09T11:05:28+08:00
draft: false
slug: "VS-Code-Terminal-nvm-complaining-prefix-option"
---

升级 macOS Mojave 新系统后，发现在 VS Code 启动终端时，每次都会提示 "prefix" 错误：

```zsh
nvm is not compatible with the npm config "prefix" option: currently set to "/usr/local"
Run `npm config delete prefix` or `nvm use --delete-prefix v8.9.1 --silent` to unset it
```

解决办法：

首先需要找到老的 node_modules 的位置：

```zsh
ls -la /usr/local/bin | grep npm
```

终端会列出路径：

```zsh
lrwxr-xr-x    1 mac   admin        46  8 29 15:40 npx -> /usr/local/lib/node_modules/npm/bin/npx-cli.js
```

然后我们删除这些文件：

```zsh
rm -R /usr/local/bin/npm /usr/local/lib/node_modules/npm/bin/npm-cli.js
```

随后发现终端又报另一个兼容性问题，因为我在使用 nvm 之前用 Homebrew 安装过 node，所以我们需要删除之前使用 Homebrew 安装的 node：

```zsh
brew uninstall --ignore-dependencies node
```

然后在 VS Code 里启动终端就不会报 "prefix" 相关的错误了。

> 参考文档：[vscode-docs](https://github.com/Microsoft/vscode-docs/blob/master/docs/editor/integrated-terminal.md#why-is-nvm-complaining-about-a-prefix-option-when-the-integrated-terminal-is-launched)