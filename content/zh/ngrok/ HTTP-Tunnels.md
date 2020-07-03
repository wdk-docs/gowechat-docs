---
title: "HTTP 隧道"
draft: false
weight: 22
type: "docs"
description: >
  HTTP Tunnels
---

## 自定义子域名

ngrok assigns random hexadecimal names to the HTTP tunnels it opens for you. This is okay for one-time personal uses. But if you're displaying the URL at a hackathon or integrating with a third-party webhook, it can be frustrating if the tunnel name changes or is difficult to read. You can specify a custom subdomain for your tunnel URL with the `-subdomain` switch.

##### 例如：打开具有隧道子站点的“inconshreveable”

    ngrok http -subdomain=inconshreveable 80


    ngrok by @inconshreveable

    ...
    Forwarding                    http://inconshreveable.ngrok.io -> 127.0.0.1:80
    Forwarding                    https://inconshreveable.ngrok.io -> 127.0.0.1:80

## 密码保护您的隧道

Anyone who can guess your tunnel URL can access your local web server unless you protect it with a password. You can make your tunnels secure with the `-auth` switch. This enforces HTTP Basic Auth on all requests with the username and password you specify as an argument.

##### Example: Password-protect your tunnel

    ngrok http -auth="username:password" 8080

## Tunnels on custom domains (white label URLs)

Instead of your tunnel appearing as a subdomain of `ngrok.io`, you can run ngrok tunnels over your domains. To run a tunnel over `dev.example.com`, follow these steps:

1. Navigate to the [Domains tab of your ngrok.com dashboard][7] and click 'Add a domain'. Enter `dev.example.com` as a Reserved Domain. This guarantees that no one else can hijack your domain name with their own tunnel.
2. On your dashboard, click on the 'CNAME' icon to copy your CNAME target.![][8]
3. Create a DNS CNAME record from `dev.example.com` to your CNAME target. In this example, we would point the CNAME record to `2w9c34maz.cname.ngrok.io`
4. Invoke ngrok with the `-hostname` switch and specify the name of your custom domain as an argument. Make sure the `-region` you specify matches the region in which you reserved your domain.

##### Example: Run a tunnel over a custom domain

        ngrok http -region=us -hostname=dev.example.com 8000

Accessing custom domain tunnels over HTTPS will still work, but the certificate will not match. If you have a TLS certificate/key pair, try using a TLS tunnel.

## Local HTTPS servers

ngrok assumes that the server it is forwarding to is listening for unencrypted HTTP traffic, but what if your server is listening for encrypted HTTPS traffic? You can specify a URL with an `https://` scheme to request that ngrok speak HTTPS to your local server.

##### Forward to an https server by specifying the https:// scheme

    ngrok http https://localhost:8443

As a special case, ngrok assumes that if you forward to port 443 on any host that it should send HTTPS traffic and will act as if you specified an `https://` URL.

##### Forward to the default https port on localhost

ngrok assumes that your local network is private and it **does not do any validation of the TLS certificate presented by your local server**.

When forwarding to a local port, ngrok does not modify the tunneled HTTP requests at all, they are copied to your server byte-for-byte as they are received. Some application servers like WAMP and MAMP and use the `Host` header for determining which development site to display. For this reason, ngrok can rewrite your requests with a modified Host header. Use the `-host-header` switch to rewrite incoming HTTP requests.

If `rewrite` is specified, the `Host` header will be rewritten to match the hostname portion of the forwarding address. Any other value will cause the `Host` header to be rewritten to that value.

##### Rewrite the Host header to 'site.dev'

    ngrok http -host-header=rewrite site.dev:80

##### Rewrite the Host header to 'example.com'

    ngrok http -host-header=example.com 80

## Serving local directories with ngrok's built-in fileserver

ngrok can serve local file system directories by using its own built-in fileserver, no separate server needed! You can serve files using the `file://` scheme when specifying the forwarding URL.

**All paths must be specified as absolute paths**, the `file://` URL scheme has no notion of relative paths.

##### Share a folder on your computer with authentication

    ngrok http -auth="user:password" file:///Users/alan/share

File URLs can look a little weird on Windows, but they work the same:

##### Share a folder on your Windows computer

    ngrok http "file:///C:UsersalanPublic Folder"

## Tunneling only HTTP or HTTPS

By default, when ngrok runs an HTTP tunnel, it opens endpoints for both HTTP and HTTPS traffic. If you wish to only forward HTTP or HTTPS traffic, but not both, you can toggle this behavior with the `-bind-tls` switch.

##### Example: Only listen on an HTTP tunnel endpoint

    ngrok http -bind-tls=false site.dev:80

##### Example: Only listen on an HTTPS tunnel endpoint

    ngrok http -bind-tls=true site.dev:80

## Disabling Inspection

ngrok records each HTTP request and response over your tunnels for inspection and replay. While this is really useful for development, when you're running ngrok on production services, you may wish to disable it for security and performance. Use the `-inspect` switch to disable inspection on your tunnel.

##### Example: An http tunnel with no inspection

    ngrok http -inspect=false 80

## Websockets

Websocket endpoints work through ngrok's http tunnels without any changes. However, there is currently no support for introspecting them beyond the initial 101 Switching Protocols response.
