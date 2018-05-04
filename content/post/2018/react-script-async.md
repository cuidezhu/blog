---
title: "React 异步引入外部js"
date: 2018-05-04T18:47:26+08:00
draft: false
slug: react-script-async
---

## 起因

最近在做项目时，需要用到第三方的人机验证码服务，而这项服务没有npm包，所以就需要引入 `<script>`，我们只需要在需要在用到这个人机验证服务的component里加入以下代码就可以实现异步加载 `<script>`：

```
componentDidMount () {
  const script = document.createElement("script")

  script.src = "//captcha.luosimao.com/static/dist/api.js"
  script.async = true

  document.body.appendChild(script)
}

```

## 更进一步

你有没有思考过为啥引入外部文件时经常省略 `http:` 或者 `https:` 呢，其实这样做是有原因的：

`//captcha.luosimao.com/static/dist/api.js` 这种写法是相对路径写法，浏览器会自动加上 `http:` 或者 `https:`补全为绝对路径，补全原则是与我们当前页面使用的协议相同。


