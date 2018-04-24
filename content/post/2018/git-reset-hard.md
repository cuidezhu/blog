---
title: "Git 回滚到某一版本"
date: 2018-04-25T00:32:03+08:00
draft: false
slug: git-reset-hard
---

## 起因

这两天写练习项目时用了eslint, 然后发现 `yarn add eslint` 或者 `yarn global add eslint` 无效，只能使用 `npm install -g eslint` 来按安装， `eslint --init`才会在命令行里起作用，随后发现带来了一些问题，并且感觉Eslint并不是理想的代码规范方式，而且还有丑陋的错误误报，遂决定暂时弃用 Eslint，卸载Eslint后，项目跑的时候总是时不时奇怪的就因为Eslint跑不起来了，所以决定把代码回滚到使用Eslint以前的版本。

## 显示提交的log

```
$ git log -5
commit 2497b4715fad2f022e5fee3e83341b19d7fa8bf7 
Author: xxx <xxx@xxx.com>
Date:   Mon Apr 23 01:26:54 2018 +0800

    aaa

commit 23faaf5ba3b5104c3f93275d9441439d31e06a74
Author: xxx <xxx@xxx.com>
Date:   Sun Apr 22 21:09:42 2018 +0800

    bbb

commit a0c8880200f8de959a12686faa7ce3b2d37b24b4
Author: xxx <xxx@xxx.com>
Date:   Sun Apr 22 15:27:55 2018 +0800

    ccc

commit b736de663899dc24d362d93ebb32f7548402cc5b
Author: xxx <xxx@xxx.com>
Date:   Sat Apr 21 23:19:45 2018 +0800

    ddd

commit ee7c7a7a6c3bc7f2698b2a0b4b1bcbe0dce93722
Author: xxx <xxx@xxx.com>
Date:   Wed Apr 18 16:56:53 2018 +0800

    eee
```

## 回滚到某个版本

复制上面log信息中你想回到某个 `commit` 后面的那串字符串

```
git reset --hard ee7c7a7a6c3bc7f2698b2a0b4b1bcbe0dce93722
```

## 强制提交

```
git push -f origin master
```

