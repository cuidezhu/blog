---
title: "Git 合并某个分支下的某个文件"
date: 2018-04-23T23:22:52+08:00
draft: false
slug: git-merge-branch-file
---

## 查看远程分支

```
git branch -r
```

## 拉取远程分支并创建本地分支

```
git checkout -b 本地分支名x origin/远程分支名x
```

## 合并分支的某个文件

现在我们想把我们刚创建的本地分支 `x` 的某个文件 `file` 或者文件夹 `folder` 内的文件合并到本地开发的分支 `develop` 上，而不是把整个分支都合并过来，方法如下：

先切换到分支 `develop` 上
```
git checkout develop
```

然后执行
```
git checkout --patch A file
```

其中 `file` 可以换成某个文件夹 `folder`，然后根据自己的需要按 `y` 或者 `n`。


