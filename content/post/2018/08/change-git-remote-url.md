---
title: "更改 Git 远程仓库地址"
date: 2018-08-13T00:09:13+08:00
draft: false
slug: "change-git-remote-url"
---

我们有时候需要更改 Git 仓库的远程地址，该如何操作呢？通常有下面几种方式：

## 先删后加

```zsh
git remote rm origin
git remote add origin git@xxx
```

