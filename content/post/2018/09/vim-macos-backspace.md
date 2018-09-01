---
title: "解决 macOS 下 Vim 的 退格键（backspace）不能用的问题"
date: 2018-09-01T22:12:13+08:00
draft: false
slug: "vim-macos-backspace"
---

发现升级到 Vim 8 之后，我是在 iTerm2 里打开的终端里的 Vim，发现退格键不能正常使用了，解决方法如下，我们要在 Vim 的配置文件 `.vimrc` 中加入下面两项配置：

```zsh
set nocompatible
set backspace=indent,eol,start
```

然后在终端执行 `source ~/.vimrc` 使修改的配置立即生效。这样就解决了退格键不起作用的问题。
