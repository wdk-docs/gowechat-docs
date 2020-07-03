---
title: "故障排除"
draft: false
weight: 212
type: "docs"
description: >
  Troubleshooting
---

## CORS with HTTP basic authentication

Yes, but you cannot use ngrok's `-auth` option. ngrok's http tunnels allow you to specify basic authentication credentials to protect your tunnels. However, ngrok enforces this policy on _all_ requests, including the preflight `OPTIONS` requests that are required by the CORS spec. In this case, your application must implement its own basic authentication. For more details, see [this github issue][29].
