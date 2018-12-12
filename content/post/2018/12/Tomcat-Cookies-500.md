---
title: "前后端联调因 cookie 问题导致 Tomcat 容器报错从而接口返回 500 错误"
date: 2018-12-11T23:54:52+08:00
draft: false
slug: "Tomcat-Cookies-500"
---

最近接手了一个别人写的系统，发现所有有和后端接口交互的页面接口都会返回 500 错误。

经过一番排查，最终发现这个系统最近加了个登陆逻辑，这个登陆是有和后端交互的，这个系统登陆当时是这样处理的，前端向后端发请求，后端返回一些字段，前端根据这些字段来设置 cookie，其中后端返回的一个字段为中文，而前后端都没对中文做处理，导致 cookie 的格式不对，所有的接口再向后端发请求时，Tomcat 容器那里会报 cookie 错误，导致请求返回 500。其实应该后端设置 cookie 的，前端设置 cookie 还得一个一个得设置。

原因是 cookie 中不该有中文，有中文等特殊字符 Tomcat 容器那里会报 cookie 错误，可以把中文编码下再保存成 cookie，前后端都可以编码，JS 这里的处理是通过 encodeURIComponent(URIstring)对 URL 进行编码，decodeURIComponent() 函数可对 encodeURIComponent() 函数编码的 URI 进行解码。
