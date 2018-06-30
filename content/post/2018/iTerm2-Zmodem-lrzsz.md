---
title: "iTerm2 使用 lrzsz 在本机和服务器之间传输文件"
date: 2018-07-01T02:05:00+08:00
draft: false
slug: "iTerm2-Zmodem-lrzsz"
---

我们有时需要在本机和服务器之间传输文件，我们可以通过 `scp` 命令来传输，但 `scp` 还是比较麻烦。也可以通过 ftp 来做，但首选我们需要在服务器上搭建 ftp 服务，也是比较麻烦。如果我们是一个自动化部署的项目，自然可以用我们选用的自动化部署方案来进行部署。但有些时候我们只是比如传某个与项目无关文件，这时我们可以用 `lrzsz` 来简单地通过 `rz` 和 `sz` 命令还进行本机和服务器之间的文件传输。

## 服务器

我服务器使用的是 CentOS 7 系统，这里我通过 `yum` 命令来安装 `lrzsz`:

```sh
yum install lrzsz
```

## 本机

如果你使用的是 Windows，你可以用 Xshell 来登录远程服务器，由于 Xshell 支持 ZModem 协议，所以什么都不用做就可以直接通过 `rz` 和 `sz` 命令来传文件了。

如果你使用的是 macOS，并且用了 item2终端，由于 iTerm2 本身不支持 ZModem 协议，所以我们可以通过 [iterm2-zmodem](https://github.com/mmastrac/iterm2-zmodem) 来让 iTerm2 支持 ZModem 协议。

### 安装 lrzsz

首先，客户端也需要安装 lrzsz

```sh
brew install lrzsz
```

### 配置 iTerm2

下载并安装脚本

```sh
git clone https://github.com/mmastrac/iterm2-zmodem.git
cd iterm2-zmodem
cp iterm2-recv-zmodem.sh iterm2-send-zmodem.sh  /usr/local/bin/
```

设置 iTerm2 上的触发器

打开 iTerm2，`Preferences` -> `Profiles` -> `Advanced` -> `Triggers` -> `Edit`，然后添加下面两个触发器：

```sh
    Regular expression: rz waiting to receive.\*\*B0100
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-send-zmodem.sh
    Instant: checked

    Regular expression: \*\*B00000000000000
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
    Instant: checked
```

然后就可以传输文件了：发送文件到服务器是用 `rz` 命令，然后选择文件即可；把服务器文件传输到本地是用 `sz filename` 命令，选择目录保存即可。