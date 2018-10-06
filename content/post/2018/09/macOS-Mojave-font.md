---
title: "升级 macOS Mojave 后部分软件(如 VS Code)字体变虚 及应用白边解决办法"
date: 2018-09-26T14:19:34+08:00
draft: false
slug: "macOS-Mojave-font"
---

## 字体变虚

升级 macOS Mojave 新系统后，苹果默认关闭了子像素抗锯齿，导致字体变细锯齿增多。像 VS Code 之类的代码编辑器显示的字体效果会变细变模糊很多，我一开始通过把编辑器的字体加粗来暂时解决，可是这样侧边栏之类的字体还是看着过细，可以通过如下方式来解决这个问题。

解决字体渲染过细，打开终端，输入：

```zsh
defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO
```

回车，然后重启应用就发现字体变回升级之前的效果了，不再需要手动为编辑器的字体加粗。

## 应用白边

第三方深色 UI 应用顶部有一条白边，打开终端，输入：defaults write -app 应用名 NSRequiresAquaSystemAppearance -bool No 回车，如：

```zsh
defaults write -app Google\ Chrome NSRequiresAquaSystemAppearance -bool No
```

针对有空格的应用名，如 Visual Studio Code，应使用 \ 来分割：Visual\ Studio\ Code

之后重启对应应用即可 （该指令相当于让应用强行使用深色模式 UI，如果应用 /系统本身是浅色的，就没必要执行这个指令）