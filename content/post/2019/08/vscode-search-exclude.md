---
title: "VS Code 搜索排除"
date: 2019-08-07T11:39:03+08:00
draft: false
slug: "vscode-search-exclude"
---

我们在使用 VS Code 的全局搜索时，比如全局搜索一个前端项目，竟然会带上打包后的 `/dist`, `/node_modules` 等目录一起搜索，这样会带来很多无关的搜索结果，而且还会让 VS Code 程序卡死，我们可以使用 VS Code 的搜索排除功能来解决这个问题。

我们可以以一个项目为中心来决定搜索时排除哪些文件夹，打开 Workspace 的 settings.json 文件，在该文件中加入下列内容：

```json
{
    // 搜索排除
    "search.exclude": {
        "**/.git": true,
        "**/node_modules": true,
        "**/bower_components": true,
        "**/tmp": true
    },
}
```

然后确保您没有关闭搜索排除功能。 在搜索区域中，展开“要排除的文件”输入框，并确保选中齿轮图标。

再次搜索时，就会发现搜索结果干净了，没有干扰结果啦。
