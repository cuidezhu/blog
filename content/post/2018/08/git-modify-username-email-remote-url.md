---
title: "Git 修改提交的用户名、邮箱和远程仓库地址"
date: 2018-08-23T23:40:58+08:00
draft: false
slug: "git-modify-username-email-remote-url"
---

Git 修改当前项目的用户名的命令：`git config user.name 你的目标用户名;`

Git 修改当前项目提交邮箱的命令：`git config user.email 你的目标邮箱名;`

改完后进入项目的 `.git` 文件夹，我们可以发现 `config` 文件已经加上了我们设置的 `user` 信息，我们也可以编辑这个文件来修改用户名和密码。

Git 修改全局的用户名和邮箱的命令为：

```zsh
git config  --global user.name 你的目标用户名；
git config  --global user.email 你的目标邮箱名;
```

也可以编辑全局的 .gitconfig 文件 `vi ~/.gitconfig` 来修改。
