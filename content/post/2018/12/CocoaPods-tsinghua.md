---
title: "CocoaPods 使用清华源"
date: 2018-12-07T23:51:42+08:00
draft: false
slug: "CocoaPods-tsinghua"
---

首先我们根据官网方法安装 CocoaPods：

```zsh
sudo gem install cocoapods
```

然后：

```zsh
cd ~/.cocoapods/repos 
pod repo remove master
git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git master
```

最后进入自己的工程，在自己工程的podFile第一行加上：

```zsh
source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
```

使用 `pod repo` 可以查看 URL 字段判断当前自己用的哪个源。