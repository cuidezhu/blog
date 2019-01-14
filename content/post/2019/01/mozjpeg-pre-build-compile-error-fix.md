---
title: "mozjpeg 编译错误解决"
date: 2019-01-14T14:57:46+08:00
draft: false
slug: "mozjpeg-pre-build-compile-error-fix"
---

在对某个项目执行 `npm  install` 时出现如下错误：

```zsh
> mozjpeg@4.1.1 postinstall /Users/dezhu/Code/wrw/node_modules/mozjpeg
> node lib/install.js

  ⚠ The `/Users/dezhu/Code/wrw/node_modules/mozjpeg/vendor/cjpeg` binary doesn't seem to work correctly
  ⚠ mozjpeg pre-build test failed
  ℹ compiling from source
  ✖ Error: autoreconf -fiv && ./configure --disable-shared --prefix="/Users/dezhu/Code/wrw/node_modules/mozjpeg/vendor" --bindir="/Users/dezhu/Code/wrw/node_modules/mozjpeg/vendor" --libdir="/Users/dezhu/Code/wrw/node_modules/mozjpeg/vendor" && make --jobs=12 && make install --jobs=12
Command failed: autoreconf -fiv
/bin/sh: autoreconf: command not found

    at ChildProcess.exithandler (child_process.js:294:12)
    at ChildProcess.emit (events.js:182:13)
    at maybeClose (internal/child_process.js:962:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:251:5)
```

经过搜索，发现可以通过如下方式解决。

首先更新 Xcode Command Line Tools 到最新版本，然后执行下面这几条命令：

```zsh
brew install libtool automake autoconf nasm
rm -rf node_modules
npm cache clean --force
npm install
```

然后就会发现 `compiling from source` 这一步骤不再报错了，我们可以正确地执行 `npm  install` 了。Netwide Assembler （简称 NASM）是一款基于英特尔 x86 架构的汇编与反汇编工具。

