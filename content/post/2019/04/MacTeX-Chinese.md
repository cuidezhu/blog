---
title: "为 MacTeX 配置中文支持"
date: 2019-04-03T23:44:13+08:00
draft: false
slug: "MacTeX-Chinese"
---

MacTeX 自带了名为 TeXShop 的编辑器，我们打开 TeXShop，然后打开偏好设置，把编码改为 UTF-8，

![UTF-8](http://static.intj.top//20190403235232.png)

然后切换到排版，把默认指令选为“采用键入的指令”，

![Command](http://static.intj.top//20190403235515.png)

我们在写带有中文的文档时，在 `.tex` 文档中加入 `\usepackage{xeCJK}` 就能处理中文了。