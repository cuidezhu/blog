---
title: "在手机端打开 PC 端启动的本地项目"
date: 2018-11-30T19:38:04+08:00
draft: false
slug: "phone-localhost"
---

# 查看局域网 IP

ifconfig en0

显示网卡 en0 的信息，找到 `inet` 字段，这个字段后面的 `ip` 地址就是你的局域网的 `ip`：

![ifconfig en0](https://static.intj.top/20190214151514.jpg)

然后可以在 PC 上启动你的项目，PC 上的打开地址比如是 `http://localhost:3000/`，就可以在移动端浏览器输入你的局域网 `ip` 地址跟上端口号就可以打开 PC 端启动的项目了，比如我这里移动端应该打开的网址是 `http://192.168.0.147:3000/`，注意你的电脑和手机应该链接同一个 `ip`，否则不是在同一个局域网内导致打不开网址。
