---
title: "更多隧道选项"
draft: false
weight: 25
type: "docs"
description: >
  More Tunneling Options
---

## Wildcard domains

ngrok permits you to bind HTTP and TLS tunnels to wildcard domains. All wildcard domains, even those that are subdomains of `ngrok.io` must first be reserved for your account on your dashboard. When using `-hostname` or `-subdomain`, specify a leading asterisk to bind a wildcard domain.

##### Bind a tunnel to receive traffic on all subdomains of `example.com`

    ngrok http --region=us --hostname *.example.com 80

## Wildcard domain rules

The use of wildcard domains creates ambiguities in some aspects of the ngrok.com service. The following rules are used to resolve these situations and are important to understand if you are using wildcard domains.

For the purposes of example, assume you have reserved the address `*.example.com` for your account.

- Connections to nested subdomains (e.g. `foo.bar.baz.example.com`) will route to your wildcard tunnel.
- You may bind tunnels on any valid subdomain of `example.com` without creating an additional reserved domain entry.
- No other account may reserve `foo.example.com` or any other subdomain that would match a wildcard domain reserved by another account.
- Connections are routed to the most specific matching tunnel online. If you are running tunnels for both `foo.example.com` and `*.example.com`, requests to `foo.example.com` will always route to `foo.example.com`

## Forwarding to servers on a different machine (non-local services)

ngrok can forward to services that aren't running on your local machine. Instead of specifying a port number, just specify a network address and port instead.

##### Example: Forward to a web server on a different machine

    ngrok http 192.168.1.1:8080
