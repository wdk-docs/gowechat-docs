---
title: "全球基础设施"
draft: false
weight: 29
type: "docs"
description: >
  ngrok运行全球分布式隧道服务器在世界各地，实现快速，低延迟的流量到您的应用程序。
---

### Locations

ngrok runs tunnel servers in datacenters around the world. The location of the datacenter within a given region may change without notice (e.g. the European servers may move from Frankfurt to London).

- ##### us - United States (Ohio)
- ##### eu - Europe (Frankfurt)
- ##### ap - Asia/Pacific (Singapore)
- ##### au - Australia (Sydney)
- ##### sa - South America (Sao Paulo)
- ##### jp - Japan (Tokyo)
- ##### in - India (Mumbai)

### Usage

**If you do not explicitly pick a region, your tunnel will be hosted in the default region, the United States**. Picking the region closest to you is as easy as specifying setting the `-region` command line flag or setting the `region` property in your configuration file. For example, to start a tunnel in the Europe region:

    ngrok http -region eu 8080

Reserved domains and reserved addresses are allocated for a specific region (the US region by default). When you reserve a domain or address, you must select a target region. You may not bind a domain or address reserved in another region other than the one it was allocated for. Attempting to do so will yield an error and prevent your tunnel session from initializing.

### Limitations

**An ngrok client may only be connected a single region**. This may change in the future, but at the moment a single ngrok client cannot host tunnels in multiple regions simultaneously. Run multiple ngrok clients if you need to do this.

**A domain cannot be reserved for multiple regions simultaneously.** It is not possible to geo-balance DNS to the same tunnel name in multiple regions. Use region-specific subdomains or TLDs if you need to do this (`eu.tunnel.example.com`, `us.tunnel.example.com`, etc).
