---
title: "zsh 遇到 permission 警告信息"
date: 2018-12-20T22:45:49+08:00
draft: false
slug: "zsh-permission"
---

今天在一台新的电脑上安装 zsh 和 oh-my-zsh 时遇到刚打开时出现如下警告信息：

```
[oh-my-zsh] Insecure completion-dependent directories detected:
drwxr-xr-x  3 501  admin   96  8 13 14:09 /usr/local/share/zsh
drwxr-xr-x  6 501  admin  192  8 13 14:23 /usr/local/share/zsh/site-functions
lrwxr-xr-x  1 501  admin   39  8 13 14:18 /usr/local/share/zsh/site-functions/_brew -> ../../../Homebrew/completions/zsh/_brew
lrwxr-xr-x  1 501  admin   44  8 13 14:18 /usr/local/share/zsh/site-functions/_brew_cask -> ../../../Homebrew/completions/zsh/_brew_cask
lrwxr-xr-x  1 501  admin   56  8 13 14:23 /usr/local/share/zsh/site-functions/_git -> ../../../Cellar/git/2.18.0/share/zsh/site-functions/_git

[oh-my-zsh] For safety, we will not load completions from these directories until
[oh-my-zsh] you fix their permissions and ownership and restart zsh.
[oh-my-zsh] See the above list for directories with group or other writability.

[oh-my-zsh] To fix your permissions you can do so by disabling
[oh-my-zsh] the write permission of "group" and "others" and making sure that the
[oh-my-zsh] owner of these directories is either root or your current user.
[oh-my-zsh] The following command may help:
[oh-my-zsh]     compaudit | xargs chmod g-w,o-w

[oh-my-zsh] If the above didn't help or you want to skip the verification of
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.
```

解决方法如下：

在 ~/.zshrc 文件第一行增加这行内容 `ZSH_DISABLE_COMPFIX=true` 这行内容，然后 `source .zshrc`。记住一定要在首行添加，我一开始在文件最后一行添加就没效果。
