---
title: "SSH 网关"
draft: false
weight: 210
type: "docs"
description: >
  **This feature is in BETA. You may enoucunter bugs; please report any that you encounter!**
---

SSH reverse tunneling is an alternative mechanism to start an ngrok tunnel without even needing to download or run the ngrok client. You can start tunnels via SSH without downloading an ngrok client by running an SSH reverse tunnel command.

The SSH gateway functionality should not be confused with exposing an SSH server via ngrok. If you want to expose your own SSH server for remote access, please refer to the [documentation on TCP tunnels][22].

## Uploading a Public Key

Before you can start a tunnel via the SSH gateway, you'll need to upload your SSH public key. To upload your SSH public key, open the file `~/.ssh/id_rsa.pub` and copy its contents. Then go to [the Auth tab on your dashboard][23] and paste the contents into the SSH Key input and optionally enter a human description (like the name of your machine). You should now be able to start SSH tunnels!

##### Copy your SSH public key on Mac OS X

    cat ~/.ssh/id_rsa.pub | pbcopy

##### Add your SSH key by pasting it into the ngrok dashboard.

![][24]

## Examples

ngrok tries to honor the syntax of `ssh -R` for all of the tunnel commands in its SSH gateway. You may wish to consult `man ssh`, and the section devoted to the `-R` option for additional details. ngrok uses additional command line options to implement features that are not otherwise available via the `-R` syntax.

The following examples demonstrate how to use the SSH gateway and provide the equivalent ngrok client command to help you best understand how to achieve similar functionality.

##### Start an http tunnel forwarding to port 80

    # equivalent: `ngrok http 80`
    ssh -R 80:localhost:80 tunnel.us.ngrok.com http

##### Start an http tunnel on a custom subdomain forwarding to port 8080

    # equivalent: `ngrok http -subdomain=custom-subdomain 8080`
    ssh -R custom-subdomain.ngrok.io:80:localhost:8080 tunnel.us.ngrok.com http

##### Start an http tunnel on a custom domain with auth

    # equivalent: `ngrok http -hostname=example.com 8080`
    ssh -R example.com:80:localhost:8080 tunnel.us.ngrok.com http -auth="user:password"

##### Start a TCP tunnel

    # equivalent: `ngrok tcp 22`
    ssh -R 0:localhost:22 tunnel.us.ngrok.com tcp 22

##### Start a TCP tunnel on a reserved address

    # equivalent: `ngrok tcp --remote-addr=1.tcp.ngrok.io:24313 22`
    ssh -R 1.tcp.ngrok.io:24313:localhost:22 tunnel.us.ngrok.com tcp

##### Start a TLS tunnel

    # equivalent: `ngrok tls 8443`
    ssh -R 443:localhost:8443 tunnel.us.ngrok.com tls

##### Start a tunnel in a different region

    # equivalent: `ngrok http -region=eu 80`
    ssh -R 80:localhost:80 tunnel.eu.ngrok.com http
