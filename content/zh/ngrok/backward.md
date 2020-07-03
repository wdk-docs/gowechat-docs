---
title: "向后兼容性"
draft: false
weight: 214
type: "docs"
description: >
  ngrok makes promises about the compatibility and stability of its interfaces so that you can can confidently build integrations on top and know what changes to expect when upgrading to newer versions.
---

## Compatibility promise

- **Point Release (2.0.0 -> 2.0.1)** \- ngrok promises no breaking changes across point releases
- **Minor Version Change (2.0 -> 2.1)** \- ngrok may make small changes that break compatibility for functionality that affects very small minority of users across a minor version change.
- **Major Version Change (2.0 -> 3.0)** \- ngrok makes no promise that any interfaces are stable across a major version change.

## What interfaces are subject to the promise?

- The ngrok command line interface: the commands and their options
- The ngrok configuration file
- The ngrok client API

Anything other interface like the logging format or the web UI is not subject to any compatibility promise and may change without warning between versions.

## Changes in 2.3

If asked to forward to port 443, ngrok will now automatically forward HTTPS traffic instead of HTTP. This change would only affect you if you previously ran a server accepting unencrypted HTTP on port 443. To workaround this, you may specify an explicit http URL if you need the old behavior: `ngrok http http://localhost:443`.

If run under sudo, the ngrok client previously consulted the sudo-ing user's home directory file when looking for its default configuration file. It now consults the home directory of the assumed user. To workaround this, you may specify an explicit configuration file location with the `-config` option.

## Changes in 2.2

The ngrok client API no longer accepts `application/x-www-form-urlencoded` request bodies. In practice, this only affects the `/api/requests/http/:id` endpoint because posting to the `/api/tunnels` endpoint with this type of request body previously caused ngrok to crash.

This change was made to help protect against maliciously crafted web pages that could cause a user to inadvertently interact with their local ngrok API.

## Changes in 2.1

Behavior changes for `http` and `tls` tunnels defined in the configuration file or started via the API that do not have a `subdomain` or `hostname` property.

    tunnels:
      webapp:
      proto: http
      addr: 80

Given this example tunnel configuration, behavior will change in the following ways.

#### Old Behavior

Starts a tunnel using the name of the tunnel as the subdomain resulting in the URL `http://webapp.ngrok.io`

#### New Behavior

Starts a tunnel with a random subdomain, for example a URL like `http://d95211d2.ngrok.io`

#### How to keep the old behavior

Add a `subdomain` property with the same name as the tunnel:

    tunnels:
      webapp:
      proto: http
      addr: 80
      **subdomain: webapp**

This behavior changed in order to make it possible to launch tunnels with random domains. This was preventing the use of the configuration file and client API to free tier users.

## ngrok 1.x sunset

The ngrok 1.X service shut down on April 4, 2016. More details can be found on the [ngrok 1.x sunset announcement][31]
