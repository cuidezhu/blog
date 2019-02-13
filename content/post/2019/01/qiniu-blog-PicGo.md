---
title: "使用七牛云作为博客图床配合 PicGo 上传图片"
date: 2019-01-15T14:58:50+08:00
draft: false
slug: "qiniu-blog-PicGo"
---

## 博客图床

我的博客是搭建在 GitHub Pages 上，GitHub Pages 对每个站点默认最大空间不能超过 1 GB，而且把图片放到 GitHub Pages 里访问速度也不是非常快，还要担心有天图片放太多导致容量不够用。所以我们需要图床服务来单独放置我们的图片。

[七牛云](https://portal.qiniu.com/signup?code=3lh1xflkhgifm)是国内非常优秀的云存储服务商，我们把图片放到七牛云上访问速度也非常快，这对读者阅读我们的博客体验也会更好。

首先我们需要注册一个[七牛云](https://portal.qiniu.com/signup?code=3lh1xflkhgifm)账号。

## 实名认证

注册完成后我们需要首先[实名认证](https://portal.qiniu.com/identity/choice)才能新建存储空间，我们个人用户申请个人认证就好了，认证方式可以选支付宝或者银行转账认证，支付宝认证需要登录个人支付宝授权七牛访问您的认证信息，授权后耐心等待至认证结束, 中途不要主动关闭页面。然后填写个人信息，提交后等待审核。进入待审核状态后，会提示“您的身份认证信息已提交， 七牛将在 3 个工作日内完成审核，审核结果将通过邮件和短信通知您。”

我大概等了 30 分钟就认证通过了。

> 实名认证后，您可获得更多免费额度：10GB 永久存储空间，10GB/月 HTTP 国内流量，10GB/月 HTTP 海外流量等等。

## 新建图床

首先我们在对象存储栏目里新建一个存储空间，存储区域根据你的需要选一个，访问控制选为公开空间。

![新建存储空间](http://static.intj.top/1550037887368.jpg)

> 公开空间，可通过文件对象的 URL 直接访问。如果要使用七牛云存储的镜像存储功能，请设置空间的属性为公有。
>
> 私有空间，文件对象的访问则必须获得拥有者的授权才能访问。
>
> 公开和私有仅对空间的读文件生效，修改、删除、写入等对空间的操作均需要拥有者的授权才能进行操作。

## 绑定域名

七牛融合 CDN 测试域名（以 clouddn.com/qiniucdn.com/qiniudn.com/qnssl.com/qbox.me 结尾），每个域名每日限总流量 10GB，每个测试域名自创建起 30 个自然日后系统会自动回收，仅供测试使用。

所以我们需要绑定我们自己的域名，绑定加速的域名必须先完成在中国大陆的 ICP 备案。

在绑定域名页加速域名字段填写一个你已备案的域名的二级域名，比如 `cdn.xxx.com`，覆盖范围我选了全球。

![创建域名](http://static.intj.top/1550026052738.jpg)

点击创建然后会提示你：

> 加速域名 cdn.xxx.com 添加成功！配置部署中。
> 需要到域名解析服务商完成 CNAME 绑定，绑定后加速服务正式生效。[如何配置 CNAME](https://developer.qiniu.com/fusion/kb/1322/how-to-configure-cname-domain-name)
> CNAME: cdn.xxx.com.qiniudns.com

我们去我们购买域名的服务提供商控制台的域名解析里添加解析记录：

![CNAME 绑定](http://static.intj.top/1550026320275.jpg)

添加好 CNAME 记录后，我们去七牛云的融合 CDN -> 域名管理可以看到我们绑定的域名，现在状态为处理中，一般 10 分钟左右就成功了，等域名状态变为成功后域名就可以使用了。

![域名管理](http://static.intj.top/1550026506784.jpg)

## PicGo

新建完图床后，我们发现在七牛网页上传图片并不方便，我们需要一款可以方便上传图片的工具，这类工具通常支持拖拽上传图片，有的还支持上传快捷键，而且可以直接生成 Markdown 格式的图片链接格式，这大大方便了我们用 Markdown 写博客。目前此类优秀的工具有老牌的收费工具 iPic 和开源免费的 [PicGo](https://github.com/Molunerfinn/PicGo)，我使用的是 [PicGo](https://github.com/Molunerfinn/PicGo) 。

[PicGo](https://github.com/Molunerfinn/PicGo) 是使用 electron-vue 写的一款免费开源的图片上传工具，方便我们把图片上传至各大图床，而且界面非常精美，个人感觉比收费软件 iPic 还要好看。我们可以去 [Github](https://github.com/Molunerfinn/PicGo/releases) 上下载该软件的最新版本。

安装好之后打开会发现状态栏多了一个 Logo 图标，我们点击该图标出现的是已上传的图片列表，默认为空。然而并没有出现软件的主界面，我一开始以为是这个软件并不能用或者没有适配新版的 macOS 系统，后来又试了几次，搜索了一下使用方法，发现在状态栏多出来的该 Logo 图标 上右键就会出现菜单，然后我们选择打开详细窗口就能打开软件的主界面啦。

## PicGo 绑定七牛图床

以上的前置工作都做完了，然后我们打开 PicGo 的详细窗口，点击图床设置，选择七牛图床，然后会让我们填写一些密钥信息。我们打开七牛云的个人中心 -> 密钥管理，然后我们可以看到一些密钥信息。

![密钥管理](http://static.intj.top/1550027375039.jpg)

把 AccessKey/SecretKey 复制到 PicGo 中，设定访问网址就是我们已备案的域名，我们应该填之前在域名解析时设置的二级域名，注意添加 `http://` 前缀。

![图床设置](http://static.intj.top/1550037972134.jpg)

在 PicGo 中填好信息后，我们点击确定，然后点击设为默认图床，PicGo 提示设置成功。

然后我们测试一个图片看看是否成功，把一张图片拖动到桌面状态栏的 PicGo 的 Logo 图标上，或者拖动到 PicGo 详细窗口的上传区，我们上传一个图片，发现上传成功了，我们在详细窗口的上传区中的链接格式中点击 Markdown 就可以得到 Markdown 格式的网址了。但是我们发现我们的图片路径在域名的根路径下，比如：`http://cdn.xxx.com/a.jpg`，经查阅资料得知，七牛云存储不支持目录，我们没有办法添加目录更有效的管理文件，所以请注意不要上传名字相同的文件。

七牛云有提示：路径前缀可以用来分类文件，例如： image/jpg/your-file-name.jpg。但经过实测，上传后七牛云会自动把 `/` 改为 ':'，很是困扰，另参考文档 [如何在空间下创建文件夹？](https://developer.qiniu.com/kodo/kb/1705/how-to-create-the-folder-under-the-space)和 [如何避免用户上传同名文件](https://developer.qiniu.com//kodo/kb/1365/how-to-avoid-the-users-to-upload-files-with-the-same-key)。只好用日期来命名图片方便管理啦，我们截图时可以用微信截图，文件名自动就是当前时间戳格式。

好了，我们已经配置好方便的把图片上传到七牛云并且使用 Markdown 格式把图片插入到文章了，尽情的享受网站快到飞起吧。
