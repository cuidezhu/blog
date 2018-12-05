---
title: "Git Rename Branch"
date: 2018-12-05T19:13:45+08:00
draft: false
slug: "git-rename-branch"
---

假定现在的分支名称为 oldName，想要修改为 newName

## 本地分支重命名

```zsh
git branch -m oldName newName
```

## 远程分支重命名

1、重命名远程分支对应的本地分支

```zsh
git branch -m oldName newName
```

2、删除远程分支

```zsh
git push --delete origin oldName
```

3、把本地分支上传并且把本地分支与远程分支关联

```zsh
git push --set-upstream origin newName
```
