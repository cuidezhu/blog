---
title: "Flutter 入门教程"
date: 2018-07-31T23:43:37+08:00
draft: false
slug: "Flutter-getting-started-tutorial"
---

Flutter 是谷歌推出的一款开发移动端 App 的框架，支持 Android，iOS，和谷歌未来的新系统 Fuchsia。Flutter 解决了 React Native 的性能问题，现在学习 Flutter 正当其时，用一套代码即可支持多平台应用，为我们带来了开发效率的极大提升。

Flutter 是用 Dart 语言写的，坊间相传 Dart 开发组就在 Flutter 团队旁边，所以 Flutter 团队为了方便才用 Dart 的，其实主要是因为 Dart 语言的高性能，Dart 支持即时编译（just-in-time compilation）和预先编译（ahead-of-time Compilation），即时编译使 Flutter 能在 App 运行时直接重新编译，热加载（Hot Reloading）带来了开发效率的提高；预先编译意味着你的 App 使用的库和函数等直接编译为原生的 ARM 代码，这时发布版的 App 运行效率更高和可预测，  所以 Dart 非常适合移动开发。而且 Dart 有静态类型，随着项目规模的增大，可以帮助你更好的追踪 Bug 和管理代码。

## 安装

Dart SDK 与 Flutter 捆绑在一起，不需要单独安装Dart。我本机的系统是 macOS，如果你是别的系统，可以去官网查看安装方法。

首先我们去[官网](https://flutter.io/setup-macos/)下载最新的安装包，当前是 v0.5.1-beta 版，然后解压到你想要的文件夹中，我是解压到了自己的家目录 `~` 下。

然后把 flutter 永久性加入到环境变量（PATH）中，打开命令行，我使用的是 iTerm2 + zsh，查看你的 flutter 的解压路径 `pwd`，然后 `vim $HOME/.zshrc` 在文件结尾加入下面这行代码：

```sh
export PATH=[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin:$PATH
```

因为我使用的是 zsh，所以我修改的是 `.zshrc` 文件，如果你用的是 bash，需要修改 `.bash_profile` 文件 `vim $HOME/.bash_profile`。

然后执行 `source $HOME/.zshrc` 使修改生效。

运行 `flutter doctor` 查看是否有需要安装的依赖，这条命令检查你的环境然后会把问题列在命令行中，根据提示安装所需依赖即可。安装完了依赖项，再次运行 `flutter doctor` 来确保一切都正常工作。

## iOS 设置

首先下载Xcode，可以去 Mac App Store 下载最新版的 Xcode，然后打开模拟器 `open -a Simulator`，在 Flutter 项目的根目录下执行 `flutter run` 即可启动 App。

一般你把 `flutter doctor` 列出的问题按照上面提示的方式解决完，就完成了发布到 iOS 和 Android 设备上的设置，具体可参考官网和 `flutter doctor` 列出的信息。

## Android 设置

首先我们安装 Android Studio，然后去 Android Studio 的 Plugins 下的Browse repositories 里搜索安装 Dart 和 Flutter 这两个插件。

然后我们来创建一个安装模拟器，启动 Android Studio>Tools>AVD Manager 选择 Create Virtual Device，选择一个设备，这里我们选择的是 Pixel 2，点 Next，选择一个系统镜像，我选的是 API 27 的 Oreo，点击 Download，选中我们下载的系统镜像，点 Next。

## VS Code 开发环境

我是用的 VS Code 编辑器，我们需要安装 flutter 这个扩展来获得更好的开发体验，直接在 VS Code 的扩展里搜索 flutter 安装即可。

然后点击 VS Code 的菜单项 `查看>命令面板`，输入 `doctor`，选择 `Flutter: Run Flutter Doctor`，然后弹出 flutter SDK 错误弹窗，我们选择设置路径，一步步选择 `[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin` 路径即可，然后就可以在 VS Code 集成的终端输出中看到 `flutter doctor` 的执行结果了。

## 创建新 Flutter 项目

Android Studio 安装完 Dart 和 Flutter 这两个插件后我们重新启动 Android Studio 发现欢迎面板里多了 `Start a new Flutter project` 这个选项，我们可以直接创建 Flutter 项目。

也可以在终端中，在你想要放置 Flutter 项目的目录执行以下命令来创建新的 Flutter 项目。

```sh
flutter create myapp
```

在创建好的项目里，`lib/main.dart` 是我们的代码文件。

## 启动项目