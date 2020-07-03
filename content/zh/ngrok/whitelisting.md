---
title: "IP白名单隧道接入"
draft: false
weight: 28
type: "docs"
description: >
  You may whitelist access to tunnel endpoints on your account. The whitelist is enforced by the ngrok.com servers. It is applied globally to all of your tunnel endpoints. Any incoming connection to any of your tunnel endpoints is checked to guarantee that the source IP address of the connection matches at least one entry in your whitelist. If a connection does not match the whitelist it is terminated immediately and never forwarded to an ngrok client.
---

As a special case, **if your whitelist is empty, all connections are allowed**.

## Managing the whitelist

You can manage the IP whitelist on the [auth tab of your ngrok dashboard][20]. Enter a new IP address under the "IP Whitelist" section and then click **Add Whitelist Entry**. Changes to the IP Whitelist can take up to 30 seconds to take effect.

## IP Ranges

Sometimes, you may wish to whitelist an entire range of IPs. Instead of entering just a single IP address, you may instead specify a block of IP addresses using [CIDR notation][21]. For example, to allow all IP addresses from 10.1.2.0 to 10.1.2.255, you would add 10.1.2.0/24 to your whitelist.
