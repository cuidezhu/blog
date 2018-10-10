---
title: "Git 忽略已经提交的文件"
date: 2018-10-10T23:54:59+08:00
draft: false
slug: "git-update-index"
---

对于已经提交的文件，即使我们把这个文件(或文件夹)加到了 `.gitignore` 里，Git还是会追踪该文件。

所以我们可以用如下这条命令来取消追踪 `PATH` 文件：

```zsh
git update-index --assume-unchanged PATH
```

PATH为要忽略的文件夹或文件，文件夹的末尾需要加 / 。