---
title: "Git 子模块踩过的坑"
date: 2018-08-17T09:12:00+08:00
draft: false
slug: "git-submodule"
---

Git 子模块允许你将一个 Git 仓库当作另外一个Git仓库的子目录。这允许你克隆另外一个仓库到你的项目中并且保持你的提交相对独立。

我在一台电脑上配置好了 Git 子目录，也把 Git 子目录生成的配置文件 `.gitmodules` 提交到了 Github 上，当我在另一台电脑上 `clone` 下来上层目录时，确发现子模块不起作用。

解决方法如下就是我们需要把根目录下（我这里是 `blog`）以前生成的 `.gitmodules` 删掉，然后把子目录（我这里是 `public`） 从 `.gitignore` 文件里删除，然后重新添加 Git 子模块，最后把子模块的目录（`public`）添加到上层目录下的 `.gitignore` 文件里：

比如我的博客系统的目录是 `blog` 目录，而生成的博客网站是 `blog` 目录下的 `public` 目录，我们已经在 Github 上建立了这两个远程仓库时，我们首先把上层目录 `blog` 对应的仓库 `clone` 到本地，我的博客系统是 hugo，当我运行 `hugo` 命令时，会在 `blog` 目录下生成一个 `public` 目录，这个下的文件需要提交到另外一个仓库来当作博客的根目录。这个 `public` 目录我们是不提交到 Github 上的

我们删除以前生成的 `.gitmodules` 文件，把 `public` 从 `blog` 目录下的 `.gitignore` 文件里删掉，然后使用以下命令重新添加子模块：

```zsh
git submodule add -b master git@github.com:cuidezhu/cuidezhu.github.io.git public
```

然后把子模块的目录（`public`）添加到上层目录下的 `.gitignore` 文件里，这样我们就配置好子模块了，下次我们添加文章后重新运行 `hugo` 命令，就会重新出现 `public` 文件夹。

然后使用 `git reset HEAD public` 来把暂存区的修改撤销掉（unstage），重新放回工作区。

然后就可以在 `blog` 和 `public` 下独立提交相关内容了。