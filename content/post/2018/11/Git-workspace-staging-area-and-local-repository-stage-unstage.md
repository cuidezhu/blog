---
title: "Git 理解工作区、暂存区、版本库和 stage unstage 状态"
date: 2018-11-14T12:01:59+08:00
draft: false
slug: "Git-workspace-staging-area-and-local-repository-stage-unstage"
---

首先我们来理解下 Git 工作区、暂存区和版本库概念

- 工作区：就是你在电脑里能看到的目录。
- 暂存区：英文叫 stage, 或 index。一般存放在 ".git 目录下" 下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是 Git 的版本库。

![Git workspace](https://static.intj.top/20190214151309.jpg)
