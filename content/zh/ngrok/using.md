---
title: "使用ngrok与..."
draft: false
weight: 211
type: "docs"
description: >
  Using ngrok with …
---

## Wordpress

To make ngrok work properly with Wordpress installations you usually need to do two things:

1. You must ensure that Wordpress issues relative URLS. You can do so by installing the following plugin:

- You must ensure that Wordpress understands that it is meant to serve itself from your tunneled hostname. You can configure Wordpress to do that by modifying your `wp-config` to include the following lines:


    define('WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST']);
    define('WP_HOME', 'http://' . $_SERVER['HTTP_HOST']);

- You must also instruct ngrok to [rewrite the host header][25], like so:


    ngrok http -host-header=rewrite https://your-site.dev

## Virtual hosts (MAMP, WAMP, etc)

Popular web servers such as MAMP and WAMP rely on a technique popularly referred to as 'Virtual Hosting' which means that they consult the HTTP request's `Host` header to determine which of their multiple sites they should serve. To expose a site like this it is possible to ask ngrok to rewrite the `Host` header of all tunneled requests to match what your web server expects. You can do this by using the `-host-header` option (see: [Rewriting the Host header][26]) to pick which virtual host you want to target. For example, to route to your local site `myapp.dev`, you would run:

    ngrok http -host-header=myapp.dev 80

## Visual Studio / IIS Express

Use dproterho's visual studio extension which adds ngrok support directly into Visual Studio: [ngrok extension for Visual Studio][27]

## An outbound proxy

ngrok works correctly through an HTTP or SOCKS5 proxy. ngrok respects the standard unix environment variable `http_proxy`. You may also set proxy configuration explicitly in the ngrok configuration file:

## node.js

Use bubenshchykov's npm package for interacting with ngrok from node.js:

## Puppet

Use gabe's puppet module for installing and configuring ngrok resources and ensure the ngrok client process is running: [ngrok module for Puppet][28]
