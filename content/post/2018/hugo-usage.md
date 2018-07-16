---
title: "Hugo 的使用"
date: 2018-04-22T21:27:08+08:00
draft: false
slug: "hugo-usage"
---

## 创建一篇文章

```sh
hugo new post/2018/hugo-usage.md
```
生成的markdown文档头部信息加上 `slug` 字段表示URL链接信息

## 本地生成预览

```sh
hugo server
```

## 生成静态页到 `public` 文件夹

```sh
hugo
```

## 部署到Github Pages上

把public文件夹下的内容推送到相应的Github Pages仓库里。