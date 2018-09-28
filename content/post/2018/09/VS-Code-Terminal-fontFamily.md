---
title: "修改 VS Code 内置终端字体"
date: 2018-09-28T15:18:24+08:00
draft: false
slug: "VS-Code-Terminal-fontFamily"
---

我的终端使用的是 Zsh，并且配置了 Zsh 的一个主题，这个主题需要安装字体来支持箭头效果，在 iTerm2 中我设置了这个字体，但是 VS Code 里这个箭头还是显示乱码，解决方法：

打开 VS Code 的 `settings.json` 文件，加入下面一行，我那个主题用的是 `Meslo LG M for Powerline` 字体：

```json
"terminal.integrated.fontFamily": "Meslo LG M for Powerline",
```

这样 VS Code 内置的终端就能正确显示 Zsh 主题的箭头了。
