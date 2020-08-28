---
title: "博客切回 HTTP 访问（已重新切回 HTTPS）"
date: 2019-02-14T11:53:10+08:00
draft: false
slug: "blog-HTTP"
---

我的博客是搭建在 GitHub Pages 上的，并且使用了 Custom domain（自定义域名），一开始我在仓库的 Settings 里勾选了 Enforce HTTPS，这样即使你在浏览器里输入 `ijs.me`，也会自动访问 `https://ijs.me`。GitHub 为我们提供了免费的证书，用的是 Let’s Encrypt 提供的免费证书，免费证书有效期比较短，每隔几个月会更新一次。

我的网站引用了一些 Cloudflare 上的 CSS 和 JS 文件，Cloudflare 在国外访问速度较快，但是在国内速度就太慢了，我就把用的这几个 Cloudflare 上的 CSS 和 JS 文件放到了七牛上，我用的是七牛免费的 HTTP 流量。在 HTTPS 网站上引用这些 HTTP 开头的文件 URL 会遇到已阻止载入混合活动内容错误，导致 CSS 和 JS 文件加载失败。

所以为了方便，我把网站取消了 Enforce HTTPS，使用 HTTPS 如果是自己的证书还要担心证书到期问题，用 HTTP 访问网站更方便。

经过实测，我们在博客文章里做的一些网站站内的一些内链，原先为 HTTPS 开头的，也需要改为 HTTP，否则该文章会自动用 HTTPS 访问，引用的一些第三方网站的 HTTPS URL 则不受影响。

## 2020 年 8 月最新更新：已重新切回 HTTPS

HTTPS 是大势所趋，Chrome 浏览器针对 HTTP 网址已提示不安全。所以为图床购买了 HTTPS 流量。