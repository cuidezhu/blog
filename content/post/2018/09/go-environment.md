---
title: "Go 环境配置"
date: 2018-09-19T13:34:41+08:00
draft: false
slug: "go-environment"
---

# Go 安装

首先我们来安装 Go，Go 有多种方法安装:

## 源码安装

Go 1.5 彻底移除 C 代码，Runtime、Compiler、Linker 均由 Go 编写，实现自举，所以我们不再需要先安装 C 编译器，直接去官网找到相应的 `goVERSION.src.tar.gz` 下载源码，解压到 `$HOME` 目录，然后执行:

```zsh
cd go/src
./all.bash
```

然后我们需要设置环境变量，可以把下面的命令写进 `.bashrc` 或者 `.zshrc` 里面：

```zsh
export GOPATH=$HOME/gopath
export PATH=$PATH:$HOME/go/bin:$GOPATH/bin
```

## Go标准包安装

但是我们最好通过 Go 标准包安装，直接下载响应系统的安装包，直接点下一步就行了。

## 第三方工具安装

我们也可以通过 Homebrew 之类的第三方工具安装：

```zsh
brew update && brew upgrade
brew install go
```

# GOPATH 和工作空间

## GOPATH

从 go 1.8 开始，GOPATH 环境变量现在有一个默认值，在 Unix 上默认为 $HOME/go，如果 `$HOME` 目录下没有 `go` 这个目录则新建。

`$GOPATH` 目录约定有三个子目录：

* src 存放源代码（比如：.go .c .h .s等）
* pkg 编译后生成的文件（比如：.a）
* bin 编译后生成的可执行文件

## 代码结构

GOPATH 下的 src 目录就是开发程序的主要目录，所有的源码都是放在这个目录下面，我们一般一个目录就是一个项目，例如: `$GOPATH/src/mymath` 表示 mymath 这个应用包或者可执行应用，这个根据 package 是 main 还是其他来决定，main 的话就是可执行应用，其他的话就是应用包。

所以当新建应用或者一个代码包时都是在 src 目录下新建一个文件夹，文件夹名称一般是代码包名称。

## 编译应用

如果是新建的是应用包，源码开头是 `package 包名`，应用包有两种方式安装：

1. 只要进入对应的应用包目录，然后执行go install，就可以安装了
2. 在任意的目录执行如下代码go install mymath

安装完之后，我们可以进入如下目录

```zsh
cd $GOPATH/pkg/${GOOS}_${GOARCH}
//可以看到如下文件
mymath.a
```

调用应用包的方法是可以在可执行应用里直接 `import`：

```zsh
package main

import (
  "mymath"
  "fmt"
)

func main() {
  fmt.Printf("Hello, world.  Sqrt(2) = %v\n", mymath.Sqrt(2))
}
```

如何编译程序呢？进入该应用目录，然后执行go build，那么在该目录下面会生成一个mathapp的可执行文件

```zsh
./mathapp
```

安装该应用，进入该目录执行go install,那么在$GOPATH/bin/下增加了一个可执行文件mathapp。

## 获取远程包

```zsh
go get github.com/astaxie/beedb
```

go get 本质上可以理解为首先第一步是通过源码工具 clone 代码到 src 下面，然后执行 `go install`

# Go 命令

直接在终端键入 `go` 可以看到 Go 支持的完整命令，我们常用以下的几条命令：

1. `go build` 用于编译代码；
 * 如果是普通包，当你执行 `go build` 之后，它不会产生任何文件。如果你需要在$GOPATH/pkg下生成相应的文件，那就得执行go install。
 * 如果是 main 包，当你执行 `go build` 之后，它就会在当前目录下生成一个可执行文件。如果你需要在 `$GOPATH/bin` 下生成相应的文件，需要执行 `go install`，或者使用 `go build -o 路径/a.exe`。
2. `go clean` 用来移除当前源码包和关联源码包里面编译生成的文件，一般利用这个命令清除编译文件，然后 GitHub 提交源码；
3. `go fmt` 可以帮你格式化你写好的代码文件，使你写代码的时候不需要关心格式，你只需要在写完之后执行 `go fmt <文件名>.go`，你的代码就被修改成了标准格式；
4. `go get` 用来动态获取远程代码包；
5. `go install` 这个命令在内部实际上分成了两步操作：第一步是生成结果文件(可执行文件或者.a包)，第二步会把编译好的结果移到 $GOPATH/pkg 或者 $GOPATH/bin；
6. `godoc` 在命令行执行 godoc -http=:端口号 比如godoc -http=:8080。然后在浏览器中打开127.0.0.1:8080，你将会看到一个golang.org的本地copy版本，`godoc` 后面还可以跟上包名函数之类的，不过我不经常用；

go 还提供其它一些工具命令：

```zsh
go version 查看go当前的版本
go env 查看当前go的环境变量
go list 列出当前全部安装的package
go run 编译并运行Go程序
```

# 开发工具

## VS Code

我是在 VS Code 里写 Go 代码，首先要在扩展里搜索 Go 这个插件并安装，然后在首选项->设置->用户设置->扩展->找到 Go configuration 打开 settings.json，在这个文件中加入下面这些配置项：

```json
{
  "go.buildOnSave": true,
  "go.lintOnSave": true,
  "go.vetOnSave": true,
  "go.buildFlags": [],
  "go.lintFlags": [],
  "go.vetFlags": [],
  "go.coverOnSave": false,
  "go.useCodeSnippetsOnFunctionSuggest": false,
  "go.formatOnSave": true,
  //goimports
  "go.formatTool": "goreturns",
}
```

然后打开命令行，利用新版 Shadowsocks 选项里的 `Copy HTTP Proxy Shell Export Line` 来在命令行走代理，把复制到剪贴板的 Export 命令粘贴到命令行里执行，然后再执行如下几条命令来安装依赖包支持：

```zsh
go get -u -v github.com/nsf/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/zmb3/gogetdoc
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/lukehoban/go-outline
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/tpng/gopkgs
go get -u -v github.com/newhook/go-symbols
go get -u -v golang.org/x/tools/cmd/guru
```

至此，我们就配置好了 Go 开发环境。