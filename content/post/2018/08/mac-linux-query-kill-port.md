---
title: "Mac Linux 查看端口占用情况及关闭端口"
date: 2018-08-20T19:14:56+08:00
draft: false
slug: "mac-linux-query-kill-port"
---

## 查看端口占用情况

```zsh
lsof -i:端口号
```

比如：

```zsh
lsof -i:8081
```

输出如下：

```zsh
COMMAND     PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node      44195  mac   20u  IPv6 0xc2056b16e4568339      0t0  TCP *:sunproxyadmin (LISTEN)
node      44195  mac   28u  IPv6 0xc2056b16e456ab79      0t0  TCP localhost:sunproxyadmin->localhost:56335 (ESTABLISHED)
ReactNati 45104  mac    8u  IPv6 0xc2056b16e456a5b9      0t0  TCP localhost:56335->localhost:sunproxyadmin (ESTABLISHED)
ReactNati 45104  mac   12u  IPv6 0xc2056b16e456a5b9      0t0  TCP localhost:56335->localhost:sunproxyadmin (ESTABLISHED)
```

## 关闭端口

```zsh
kill -9 44195
kill -9 45104
```

执行上门两条命令，