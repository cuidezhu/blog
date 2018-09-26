---
title: "升级 macOS Mojave 后部分软件(如 VS Code)字体变细解决办法"
date: 2018-09-26T14:19:34+08:00
draft: false
slug: "macOS-Mojave-font"
---

升级 macOS Mojave 新系统后，苹果默认关闭了子像素抗锯齿，导致字体变细锯齿增多。像 VS Code 之类的代码编辑器显示的字体效果会变细变模糊很多，我一开始通过把编辑器的字体加粗来暂时解决，可是这样侧边栏之类的字体还是看着过细，可以通过如下方式来解决这个问题。

解决字体渲染过细，打开终端，输入：

```zsh
defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO
```

回车，然后重启应用就发现字体变回升级之前的效果了，不再需要手动为编辑器的字体加粗。