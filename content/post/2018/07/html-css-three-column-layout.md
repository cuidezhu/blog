---
title: "五种方式实现三栏布局（两边固定宽度，中间自适应）"
date: 2018-07-11T23:15:41+08:00
draft: false
slug: "Html-Css-Three-Column-Layout"
---

三栏布局是网页中常见的布局，即两边固定宽度，中间自适应，下面我们用五种不同的方式来实现这种布局。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    html * {
      margin: 0;
      padding: 0;
    }

    .layout div {
      min-height: 150px;
    }
  </style>
</head>
<body>
  <!-- float 浮动解决方案 -->
  <section class="layout float">
    <style>
      .layout.float .left {
        float: left;
        width: 300px;
        background: red;
      }
      .layout.float .right {
        float: right;
        width: 300px;
        background: blue;
      }
      .layout.float .center {
        background: yellow;
      }
    </style>
    <div class="left"></div>
    <div class="right"></div>
    <div class="center">
      <h1>浮动解决方案</h1>
      <p>这是三栏布局的中间部分</p>
    </div>
  </section>
</body>
</html>
```

## 绝对定位

## Flex 布局

## 表格布局

虽然表格布局已被淘汰，但 `display: table-cell;`

## Grid（网格）布局