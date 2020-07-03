---
title: "文档"
draft: false
weight: 21
type: "docs"
description: >
  Documentation
---

## 暴露本地 Web 服务器到互联网

ngrok allows you to expose a web server running on your local machine to the internet. Just tell ngrok what port your web server is listening on.

If you don't know what port your web server is listening on, it's probably port 80, the default for HTTP.

##### 例: 暴露在你的本地机器的端口 80 的 web 服务器到互联网

```sh
ngrok http 80
```

当您启动 ngrok，它会显示在你的终端的 UI 与隧道，大约在你的隧道建立的连接其他状态和指标信息的公开网址。

##### 该 ngrok 控制台 UI

```sh
    ngrok by @inconshreveable

    Tunnel Status                 online
    Version                       2.0/2.0
    Web Interface                 http://127.0.0.1:4040
    Forwarding                    http://92832de0.ngrok.io -> localhost:80
    Forwarding                    https://92832de0.ngrok.io -> localhost:80

    Connnections                  ttl     opn     rt1     rt5     p50     p90
                                  0       0       0.00    0.00    0.00    0.00
```

## 检查您的流量

ngrok 提供了一个实时网络用户界面，你可以内省所有的 HTTP 流量的隧道跑过来。
你已经开始 ngrok 后，只需在网络浏览器中打开检查请求的详细信息。

试着做你的公共 URL 的请求。之后，你有，回头看看检查 UI。
你会看到所有的请求和响应，包括时间，持续时间，标题，查询参数和要求的有效载荷以及电线上的原始字节的细节。

##### HTTP 请求和响应的详细反省

![][1]

## 重播请求

制定一项由外部 API 的发行通常可以通过要求你做一些工作，如拨打电话，触发钩请求放慢开发周期网络挂接。
ngrok 让你可以重播一次点击大大地加快了您的迭代周期的任何请求。
点击上卷材检查 UI 任何要求重播的右上角的**Replay**按钮。

##### 重播的任何请求对您的隧道网络服务器与点击

![][2]

## 安装您的 authToken

在进一步章节中描述的 ngrok.com 服务的许多先进的功能需要你[一个帐户注册][3].
一旦你注册，你需要配置 ngrok 用的 authToken 是在仪表板出现。
这将授予您访问仅帐户的功能。
ngrok 有一个简单的“authToken”命令来使这个容易。
引擎盖下，所有的 authToken 命令的作用是添加（或修改）`authtoken`财产在你的[ngrok 配置文件][4].

##### 安装您的 authToken

```sh
    ngrok authtoken
```

## 获得一个稳定的 URL

在免费的计划，ngrok 的 URL 是随机生成的和暂时的。 If you want to use the same URL every time, you need to upgrade to a paid plan so that you can use the [subdomain option][5] for a stable URL with HTTP or TLS tunnels and the [remote-addr option][6] for a stable address with TCP tunnels.

[1]: https://ngrok.com/static/img/inspect2.png
[2]: https://ngrok.com/static/img/replay2.png
[3]: https://dashboard.ngrok.com/signup
[4]: https://ngrok.com#config
[5]: https://ngrok.com#http-subdomain
[6]: https://ngrok.com#tcp-remote-addr
