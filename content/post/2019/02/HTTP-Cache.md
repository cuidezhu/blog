---
title: "HTTP Cache"
date: 2019-02-12T15:33:53+08:00
draft: false
slug: "HTTP-Cache"
---

## 强缓存

我们从浏览器向服务器发请求时，浏览器再次向同样的接口发送请求时，会首先根据上次请求的 Response Headers 的 cache-control 和 expires 判断是否命中强缓存，若命中则直接从缓存中获取请求结果，不再真的发请求到服务器。

如果 cache-control 与 expires 同时存在的话，cache-control 的优先级高于 expires。

## 协商缓存

若没有命中强缓存，就会发一个请求到服务器，验证协商缓存是否命中。根据 Last-Modified 和 ETag 来判断是否命中协商缓存。

### 1、Last-Modified，If-Modified-Since

浏览器第一次跟服务器请求一个资源，服务器在返回这个资源的同时，在 respone 的 header 加上 Last-Modified 的 header，这个 header 表示这个资源在服务器上的最后修改时间。

浏览器再次跟服务器请求这个资源时，在 request 的 header 上加上 If-Modified-Since 的 header，这个 header 的值就是上一次请求时返回的 Last-Modified 的值。

服务器再次收到资源请求时，根据浏览器传过来 If-Modified-Since 和资源在服务器上的最后修改时间判断资源是否有变化，如果没有变化则返回 304 Not Modified，但是不会返回资源内容；如果有变化，就正常返回资源内容。当服务器返回 304 Not Modified 的响应时，response header 中不会再添加 Last-Modified 的 header，因为既然资源没有变化，那么 Last-Modified 也就不会改变，这是服务器返回 304 时的 response header。

### 2、ETag、If-None-Match

`Etag`是由服务器生成的每个资源的唯一标识字符串，只要资源有变化就这个值就会改变。

`If-None-Match`的 header 会将上次返回的`Etag`发送给服务器，询问该资源的`Etag`是否有更新，有变动就会发送新的资源回来。与 Last-Modified 不一样的是，当服务器返回 304 Not Modified 的响应时，由于 ETag 重新生成过，response header 中还会把这个 ETag 返回，即使这个 ETag 跟之前的没有变化。

`ETag`的优先级比`Last-Modified`更高。

### 既然有 Last-Modified 为何还要 Etag

HTTP1.1 中 Etag 的出现主要是为了解决几个 Last-Modified 比较难解决的问题：

- 一些文件也许会周期性的更改，但是他的内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新 GET；

- 某些文件修改非常频繁，比如在秒以下的时间内进行修改，(比方说 1s 内修改了 N 次)，If-Modified-Since 能检查到的粒度是 s 级的，这种修改无法判断(或者说 UNIX 记录 MTIME 只能精确到秒)；

- 某些服务器不能精确的得到文件的最后修改时间。

这时，利用 Etag 能够更加准确的控制缓存，因为 Etag 是服务器自动生成或者由开发者生成的对应资源在服务器端的唯一标识符。

Last-Modified 与 ETag 是可以一起使用的，服务器会优先验证 ETag，一致的情况下，才会继续比对 Last-Modified，最后才决定是否返回 304。
