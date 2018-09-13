---
title: "修改并发布 node_modules 包"
date: 2018-09-13T14:54:58+08:00
draft: false
slug: "modify-node-modules-package"
---

如果我们要修改引用的某个 npm 包该怎么做呢，如果直接去修改 node_modules 下的代码，因为我们不应该把 node_modules 目录传到 Git 上，所以我们改了别人安装依赖时还是没修改过的代码。合理的做法是什么呢？

我们首先应该在 GitHub 上 fork 一份要修改的源代码，然后 clone 到我们本地，进行我们所需的修改，把修改提交到 GitHub 上，由于 npm 可以直接从 GitHub 上安装包，所以我们可以执行 `npm install https://github.com/<username>/<repository>/tarball/master` 来安装。另外我们还可以把我们修改后的包发布到 npm 上，这样别人也可以直接从 npm 源安装。

由于我们通常会把源换成淘宝源来获取更快的安装速度，所以我们要发布包的话需要先把源改回到官方源：

```zsh
npm config set registry https://registry.npmjs.org/
```

然后如果你已经有 npm 账户了，执行 `npm login` 来登录；如果没有账户，就执行 `npm adduser` 来创建账户。

可以执行 `npm whoami` 来查看你是用哪个账户登录的。

我们的 `package.json` 里的 `name` 字段表示包名，需要我们取一个别人没有用过的包名，并且不能和别人的太像，修改好代码后执行 `npm publish` 来发布你的包，这时你就可以在 https://www.npmjs.com 搜索到你刚发布的包了，并且过一会儿也可以在 [淘宝 NPM 镜像源](https://npm.taobao.org) 上搜索到你发布的包了，可以像其他人发布的包那样正常使用。

我们发布完我们的包后，可以把 npm 源改回淘宝源：

```zsh
npm config set registry https://registry.npm.taobao.org
```

这样我们和别人都可以用我们修改过后的 npm 包了。