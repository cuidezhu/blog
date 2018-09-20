---
title: "Create React App 通过 Proxy 在本地跨域请求 API"
date: 2018-09-20T13:47:52+08:00
draft: false
slug: "create-react-app-proxy"
---

我们通过 `create-react-app` 新建项目在本地开发时，通常需求请求数据 API，而数据 API 大多数是会遇到跨域问题的，在线上生产环境我们可以通过 Nginx 配置反向代理到我们 build 之后的项目目录，可是在本地应该怎么解决跨域问题了，当然我们在本地也可以配置 Nginx 反向代理了，但是我们的 `create-react-app` 在本地开发时已经开了一个端口启动了一个 devServer 了，所以我们配置 Nginx 反向代理的话就需要再配置一个端口，把这个端口的反向代理到 `create-react-app` 启动的这个端口上，以前的一篇文章 [Nginx 解决本地开发启动 Server 跨域问题](https://ijs.me/2018/08/31/react-nginx-server-cors/) 详细讲了这种方法，这种方法比较繁琐，而且还可能会遇到问题。

`create-react-app` 为我们提供一个可配置的 `proxy` 选项，我们只需要在 `package.json` 文件里加入这个字段并正确配置就可以了。

当我们所有的请求都只是请求同一个域名的 API 时，我们这样写：

```json
"proxy": "https://api-domain.com"
```

比如我们使用 `fetch` 发请求时，devServer 就会把本地的请求都代理到 `proxy` 指定的域名 `https://api-domain.com` 上，那么如果我们项目请求 API 有多个域名怎么办呢？

如果需要区分多个 API，我们 `proxy` 的值需要是个对象：

```json
"proxy": {
  "/aapi": {
    "target": "https://a.api-domain.com",
    "secure": false,
    "changeOrigin": true
  },
  "/bapi": {
    "target": "https://b.api-domain.com",
    "secure": false,
    "changeOrigin": true
  }
}
```

由于我们的 API 是 HTTPS 协议的，所以我们每个 API 配置需要加这两个字段：

```json
"secure": false,
"changeOrigin": true
```

我们发请求就可以直接不用跟域名直接请求接口路径了，比如 `fetch('/bapi/list')` 这样写来请求数据。

然后改完 `package.json` 后我们重启我们的项目，之后就能正确的解决 API 跨域的问题了，


