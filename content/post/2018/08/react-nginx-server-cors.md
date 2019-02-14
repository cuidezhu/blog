---
title: "Nginx 解决本地开发启动 Server 跨域问题"
date: 2018-08-31T23:54:43+08:00
draft: false
slug: "react-nginx-server-cors"
---

当我们本地使用开发项目启动了一个 server 后依然打算用 Nginx 反向代理解决 API 跨越问题时，可以这样做：比如我们本地项目的 server 启动的端口是 8000，没有跨域请求 API 时，我们直接在本地访问 http://localhost:8000/ 来访问我们的项目，当我们遇到跨域问题该怎么办呢，我们在服务器上一般通过 Nginx 反向代理到我们项目 `build` 之后的路径上，那么在我们本地应该怎么做呢？我们在本地 Nginx 可以再监听一个额外的端口，比如 8001，把这个端口的 `/` 请求代理到我们的 `http://localhost:8000/` 上，把 `/api` 请求代理到我们的 API 域名上，Nginx 配置如下：

```
server {
  listen 8001;
  server_name localhost;
  location / {
    proxy_pass http://localhost:8000;
  }
  location /api/ {
    proxy_pass http://api-domain.com;
  }
}
```

这样我们平时开发就访问 `http://localhost:8001/` 来查看我们的项目。

当我们使用 webpack 或者 webpack 基础上构建的脚手架时，更好的办法是使用 `proxy` 字段来在本地解决 API 跨域的问题，比如我的另一篇文章 [Create React App 通过 Proxy 在本地跨域请求 API](http://ijs.me/2018/09/20/create-react-app-proxy/) 介绍了使用 create-react-app 这个使用了 webpack 的脚手架配置 API 代理的方法。
