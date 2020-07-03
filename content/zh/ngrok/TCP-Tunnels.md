---
title: "TCP 隧道"
draft: false
weight: 24
type: "docs"
description: >
  Not all services you wish to expose are HTTP or TLS based. ngrok TCP tunnels allow you to expose any networked service that runs over TCP. This is commonly used to expose SSH, game servers, databases and more. Starting a TCP tunnel is easy.
---

##### Expose a TCP based service running on port 1234

## Examples

##### Expose an SSH server listening on the default port

##### Expose a Postgres server listening on the default port

##### Expose an RDP server listening on the default port

## Listening on a reserved remote address

Normally, the remote address and port is assigned randomly each time you start a TCP tunnel. For production services (and convenience) you often want a stable, guaranteed remote address. To do this, first, log in to your ngrok.com dashboard and click "Reserve Address" in the "Reserved TCP Addresses" section. Then use the `-remote-addr` option when invoking ngrok to bind a tunnel on your reserved TCP address. Make sure the `-region` you specify matches the region in which you reserved your address.

##### Bind a TCP tunnel on a reserved remote address

    ngrok tcp --region=us --remote-addr 1.tcp.ngrok.io:20301 22
