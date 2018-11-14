---
title: "iTerm2 + oh-my-zsh + agnoster 打造完美终端"
date: 2018-11-13T16:07:03+08:00
draft: false
slug: "iTerm2-oh-my-zsh-agnoster"
---

## 安装 iTerm2

首先我们安装 iTerm2，去 [iTerm2 官网](https://iterm2.com/index.html) 下载安装即可。

## 切换默认终端为 zsh

首先通过 `cat /etc/shells` 来查看当前系统可以使用哪些 shell：

```zsh
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```

通过 `echo $SHELL` 命令可以查看当前正在使用的 shell。

如果当前的 shell 不是 zsh，可以通过 `chsh -s /bin/zsh` 来把 shell 切换为 zsh，终端重启之后将生效。

## 安装 oh-my-zsh

根据 GitHub 上的 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 仓库说明的方法安装即可：

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

注意我们在终端运行需要从 GitHub 下载内容的命令时，可能需要科学上网，可以用最新版的 Shadowsocks 的 `复制终端代理命令` 选项，然后在终端里执行复制的命令，就可以在终端里走代理了，这大概是最简单的终端走代理的方法，重新打开终端时代理会失效。

## 配置 agnoster 主题

agnoster 主题依赖于 [Solarized](https://ethanschoonover.com/solarized/) 配色方案和 Meslo LG 字体。

### 安装 Solarized 配色

首先我们来安装 [Solarized](https://ethanschoonover.com/solarized/) 配色方案，先去 [Solarized 官网](https://ethanschoonover.com/solarized/) 把配色文件下载下来，然后解压下载下来的文件夹得到 solarized 文件夹，打开这个文件夹后找到 iterm2-colors-solarized 文件夹打开，双击 Solarized Dark.itermcolors 文件即可安装深色主题的配色方案，然后进入iTerm2 -> Preferences -> Profiles -> Colors -> Color Presets 选择 Solarized Dark 即可。

### 安装 Powerline fonts

首先把 Powerline fonts 仓库 clone 到本地，随意进入一个文件夹，比如 `cd ~/Documents`，然后执行：

```zsh
git clone git@github.com:powerline/fonts.git
```

然后安装字体，执行下面两条命令：

```zsh
cd fonts
./install.sh
```

安装好字体之后我们来设置 iTerm2 的字体，进入 iTerm2 -> Preferences -> Profiles -> Text，在 Font 区域点击 Change Font 按钮，然后找到并选中 Meslo LG S for Powerline字体，字体大小也可以改为 13 号字体，个人感觉 13 号字体比 12 号字体更舒服。

### 安装 agnoster 主题

最新版的 oh-my-zsh 已经自带了好多主题，可以查看 [主题预览](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes) 的列表，选一种你喜欢的，这里我们使用 agnoster 这个主题。

```zsh
vim ~/.zshrc
```

然后把 `ZSH_THEME` 字段的值改为 `ZSH_THEME="agnoster"`，修改完成后执行 `source .zshrc`，我们发现修改已经生效了。

## 安装高亮插件

我们可以直接使用 Homebrew 来安装 zsh 的语法高亮插件 [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)：

```zsh
brew install zsh-syntax-highlighting
```

并将

```zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

这一句加到你的 `~/.zshrc` 文件中的最后一行，执行 `source ~/.zshrc` 后就会发现终端已经带语法高亮效果了。

## 设置透明度和背景

首先设置透明度，进入 iTerm2 -> Preferences -> Profiles -> Window -> Window Appearance 中的 Transparency 的滑动输入条，滑到一个你喜欢的透明度即可。

然后我们设置背景图片，我这里用的是微软的 Surface Studio 的 4K 壁纸，需要可以去 [百度云](https://pan.baidu.com/s/18jNrULbr05npD0Z_ht28Kg) 下载，进入 iTerm2 -> Preferences -> Profiles -> Window -> BackGround Image 勾选 BackGround image 然后选择你下载下来的图片即可。

## 设置打开关闭 iTerm2 快捷键

进入 iTerm2 -> Preferences -> Keys 然后在 Hotkey 那里选中 `show/hide all windows with a system-wide hotkey`，然后录入你要用的快捷键，这里我用的是 `Ctrl + T` 快捷键。然后就可以通过按 `Ctrl + T` 来打开和关闭 iTerm2 了。

最终效果如下：

![iTerm2](/img/2018/11/iTerm2.png)

## 配置 VS Code


下面两行分别把 VS Code 集成的终端设为 zsh 和 把字体设为 "Meslo LG S for Powerline"：


```json
"terminal.integrated.shell.osx": "/bin/zsh",
"terminal.integrated.fontFamily": "Meslo LG S for Powerline"
```

我们把上面两行加入 VS Code 的 settings.json 然后保存就好了。