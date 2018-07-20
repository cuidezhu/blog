---
title: "在 Emacs 中搭建 Clojure Repl 环境"
date: 2018-06-02T23:34:49+08:00
draft: false
slug: "Spacemacs-Clojure-Repl"
---

## 安装 Spacemacs

首先安装 Emacs:

```sh
brew install emacs --with-cocoa
```

`--with-cocoa` 即为安装带有 GUI (图形界面)的 Emacs.

如果你之前使用过 Emacs, 家目录已经有 `.emacs.d` 的配置文件了，可以先备份：

```sh
cd ~
mv .emacs.d .emacs.d.bak
mv .emacs .emacs.bak
```

然后下载 Spacemacs 配置文件：

```sh
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

然后打开 Emacs 图形界面应用程序，这里为什么不在终端中打开 Emacs 呢，因为 Spacemacs 本身就是为 GUI 而设计的，在终端中打开的话，就连 Spacemacs 的 Logo 都会乱码。直接在终端中输入 `emacs` 打开的就是 GUI 版的 Emacs 了，然后按照提示回答三个问题，让你选择风格的，我习惯用 Vim， 三个问题我选的都是第一项。然后就可以安装了，耐心等待安装完全部的插件，Spacemacs 就安装好了。这样 Spacemacs 就可以使用 vim 的很多快捷键来使用 Emacs 了，结合了 Emacs 和 Vim 的优点。

## 安装 Leiningen

Leiningen 是用来自动化生成 Clojure 项目，官方使用的安装方法稍微麻烦，我们直接通过 Homebrew 来安装 Leiningen:

```sh
brew install Leiningen
```


