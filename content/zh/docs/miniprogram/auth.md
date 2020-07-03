---
title: "微信登录"
draft: false
weight: 1
description: >
  获取操作实例，微信登录根据 jscode 获取用户 session 信息
---

## 获取操作实例

```
mini := wc.GetMiniProgram(cfg)
a:=mini.GetAuth()
```

## 根据 jscode 获取用户 session 信息

```go
Code2Session(jsCode string) (result ResCode2Session, err error)
```
