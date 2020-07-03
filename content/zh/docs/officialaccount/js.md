---
title: "JS-SDK"
draft: false
weight: 8
description: >
  获取 js-sdk 操作实例，获取 js 配置
---

## 获取 js-sdk 操作实例

```go
oa := wc.GetOfficialAccount(cfg)
j:=oa.GetJS()
```

## 获取 js 配置

```go
GetConfig(uri string) (config *Config, err error)
```

其中 `Config` 结果为：

```go
// Config 返回给用户jssdk配置信息
type Config struct {
	AppID     string `json:"app_id"`
	Timestamp int64  `json:"timestamp"`
	NonceStr  string `json:"nonce_str"`
	Signature string `json:"signature"`
}
```

## 替换`js-ticket`取值方式

默认 js-ticket 是存放在 sdk 设置的 cache，如果需要自定义取值，可以实现`credential.JsTicketHandle`接口：

```go
//JsTicketHandle js ticket获取
type JsTicketHandle interface {
	//GetTicket 获取ticket
	GetTicket(accessToken string) (ticket string, err error)
}
```

然后通过`js.SetJsTicketHandle(ticketHandle credential.JsTicketHandle)`进行设置
