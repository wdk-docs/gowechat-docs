---
title: "该ngrok配置文件"
draft: false
weight: 26
type: "docs"
description: >
  有时候，你的ngrok配置过于复杂的命令行选项来表示。 ngrok supports an optional, **extremely simple YAML configuration file** which provides you with the power to run multiple tunnels simultaneously as well as to tweak some of ngrok's more arcane settings.
---

## Configuration file location

You may pass a path to an explicit configuration file with the `-config` option. This is recommended for all production deployments.

##### Explicitly specify a configuration file location

    ngrok http -config=/opt/ngrok/conf/ngrok.yml 8000

You may pass the `-config` option more than once. If you do, the first configuration is parsed and each successive configuration is merged on top of it. This allows you to have per-project ngrok configuration files with tunnel definitions but a master configuration file in your home directory with your authtoken and other global settings.

##### Specify an additional configuration file with project-specific overrides

    ngrok start -config ~/ngrok.yml -config ~/projects/example/ngrok.yml demo admin

## Default configuration file location

If you don't specify a location for a configuration file, ngrok tries to read one from the default location `$HOME/.ngrok2/ngrok.yml`. The configuration file is optional; no error is emitted if that path does not exist.

In the default path, $HOME is the home directory for the current user as defined by your operating system. It is **not the environment variable $HOME\*\*, although they are often the same. For major operating systems, if your username is `example` the default configuration would likely be found at the following paths:

| ----- |
| OS X | `/Users/example/.ngrok2/ngrok.yml` |
| Linux | `/home/example/.ngrok2/ngrok.yml` |
| Windows | `C:Usersexample.ngrok2ngrok.yml` |

## Tunnel definitions

The most common use of the configuration file is to define tunnel configurations. Defining tunnel configurations is useful because you may then start pre-configured tunnels by name from your command line without remembering all of the right arguments every time.

Tunnels are defined as mapping of name -> configuration under the `tunnels` property in your configuration file.

##### Define two tunnels named 'httpbin' and 'demo'

    tunnels:
      httpbin:
        proto: http
        addr: 8000
        subdomain: alan-httpbin
      demo:
        proto: http
        addr: 9090
        hostname: demo.inconshreveable.com
        inspect: false
        auth: "demo:secret"

##### Start the tunnel named 'httpbin'

Each tunnel you define is a map of configuration option names to values. The name of a configuration option is usually the same as its corresponding command line switch. Every tunnel must define `proto` and `addr`. Other properties are available and many are protocol-specific.

##### Tunnel Configuration Properties

| ----- |
| `proto` |

required

all

| tunnel protocol name, one of `http`, `tcp`, `tls` |
| `addr` |

required

all

| forward traffic to this local port number or network address |
| `inspect` |

all

| enable http request inspection |
| `auth` |

http

| HTTP basic authentication credentials to enforce on tunneled requests |
| `host_header` |

http

| Rewrite the HTTP Host header to this value, or `preserve` to leave it unchanged |
| `bind_tls` |

http

| bind an HTTPS or HTTP endpoint or both `true`, `false`, or `both` |
| `subdomain` |

http

tls

| subdomain name to request. If unspecified, uses the tunnel name |
| `hostname` |

http

tls

| hostname to request (requires reserved name and DNS CNAME) |
| `crt` |

tls

| PEM TLS certificate at this path to terminate TLS traffic before forwarding locally |
| `key` |

tls

| PEM TLS private key at this path to terminate TLS traffic before forwarding locally |
| `client_cas` |

tls

| PEM TLS certificate authority at this path will verify incoming TLS client connection certificates. |
| `remote_addr` |

tcp

| bind the remote TCP port on the given address |
| `metadata` |

all

| arbitrary user-defined metadata that will appear in the ngrok service API when listing tunnels |

## Running multiple simultaneous tunnels

You can pass multiple tunnel names to `ngrok start` and ngrok will run them all simultaneously.

##### Start three named tunnels from the configuration file

    ngrok start admin ssh metrics


    ngrok by @inconshreveable

    Tunnel Status                 online
    Version                       2.0/2.0
    Web Interface                 http://127.0.0.1:4040
    Forwarding                    http://admin.ngrok.io -> 10.0.0.1:9001
    Forwarding                    http://device-metrics.ngrok.io -> localhost:2015
    Forwarding                    https://admin.ngrok.io -> 10.0.0.1:9001
    Forwarding                    https://device-metrics.ngrok.io -> localhost:2015
    Forwarding                    tcp://0.tcp.ngrok.io:48590 -> localhost:22
    ...

You can also ask ngrok to start all of the tunnels defined in the configuration file with the `\--all` switch.

##### Start all tunnels defined in the configuration file

Conversely, you may ask ngrok to run without starting any tunnels with the `\--none` switch. This is useful if you plan to manage ngrok's tunnels entirely via the API.

##### Run ngrok without starting any tunnels

## Example Configuration Files

Example configuration files are presented below. The subsequent section contains full documentation for all configuration parameters shown in these examples.

##### Run tunnels for multiple virtual hosted development sites

    authtoken: 4nq9771bPxe8ctg7LKr_2ClH7Y15Zqe4bWLWF9p
    tunnels:
      app-foo:
        addr: 80
        proto: http
        host_header: app-foo.dev
      app-bar:
        addr: 80
        proto: http
        host_header: app-bar.dev

##### Tunnel a custom domain over both http and https with your own certificate

    authtoken: 4nq9771bPxe8ctg7LKr_2ClH7Y15Zqe4bWLWF9p
    tunnels:
      myapp-http:
        addr: 80
        proto: http
        hostname: example.com
        bind_tls: false
      mypp-https:
        addr: 443
        proto: tls
        hostname: example.com

##### Expose ngrok's web inspection interface and API over a tunnel

    authtoken: 4nq9771bPxe8ctg7LKr_2ClH7Y15Zqe4bWLWF9p
    tunnels:
      myapp-http:
        addr: 4040
        proto: http
        subdomain: myapp-inspect
        auth: "user:secretpassword"
        inspect: false

##### Example configuration file with all options

    authtoken: 4nq9771bPxe8ctg7LKr_2ClH7Y15Zqe4bWLWF9p
    region: us
    console_ui: true
    http_proxy: false
    inspect_db_size: 50000000
    log_level: info
    log_format: json
    log: /var/log/ngrok.log
    metadata: '{"serial": "00012xa-33rUtz9", "comment": "For customer alan@example.com"}'
    root_cas: trusted
    socks5_proxy: "socks5://localhost:9150"
    update: false
    update_channel: stable
    web_addr: localhost:4040
    tunnels:
      website:
        addr: 8888
        auth: bob:bobpassword
        bind_tls: true
        host_header: "myapp.dev"
        inspect: false
        proto: http
        subdomain: myapp

      e2etls:
        addr: 9000
        proto: tls
        hostname: myapp.example.com
        crt: example.crt
        key: example.key

      ssh-access:
        addr: 22
        proto: tcp
        remote_addr: 1.tcp.ngrok.io:12345

## Configuration Options

## `authtoken`

This option specifies the authentication token used to authenticate this client when it connects to the ngrok.com service. After you've created an ngrok.com account, your dashboard will display the authtoken assigned to your account.

##### ngrok.yml specifying an authtoken

    authtoken: 4nq9771bPxe8ctg7LKr_2ClH7Y15Zqe4bWLWF9p

---

## `console_ui`

| ----- |
| `true` | | enable the console UI |
| `false` | | disable the console UI |
| `iftty` |

default

| enable the UI only if standard out is a TTY (not a file or pipe) |

---

## `console_ui_color`

| ----- |
| `transparent` | | don't set a background color when displaying the console UI |
| `black` |

default

| set the console UI's background to black |

---

## `http_proxy`

URL of an HTTP proxy to use for establishing the tunnel connection. Many HTTP proxies have connection size and duration limits that will cause ngrok to fail. Like many other networking tools, ngrok will also respect the environment variable `http_proxy` if it is set.

##### Example of ngrok over an authenticated HTTP proxy

    http_proxy: "http://user:password@proxy.company:3128"

---

## `inspect_db_size`

| ----- |
| positive integers | | size in bytes of the upper limit on memory to allocate to save requests over HTTP tunnels for inspection and replay. |
| `0` |

default

| use the default allocation limit, 50MB |
| `-1` | | disable the inspection database; this has the effective behavior of disabling inspection for all tunnels |

---

## `log_level`

Logging level of detail. In increasing order of verbosity, possible values are:`crit`,`warn`,`error`,`info`,`debug`

---

## `log_format`

Format of written log records.

| ----- |
| `logfmt` | | human and machine friendly key/value pairs |
| `json` | | newline-separated JSON objects |
| `term` |

default

| custom colored human format if standard out is a TTY, otherwise same as `logfmt` |

---

## `log`

Write logs to this target destination.

| ----- |
| `stdout` | | write to standard out |
| `stderr` | | write to standard error |
| `false` |

default

| disable logging |
| other values | | write log records to file path on disk |

---

## `metadata`

Opaque, user-supplied string that will be returned as part of the ngrok.com API response to the [List Online Tunnels resource][11] for all tunnels started by this client. This is a useful mechanism to identify tunnels by your own device or customer identifier. Maximum 4096 characters.

    metadata: bad8c1c0-8fce-11e4-b4a9-0800200c9a66

---

## `region`

Choose the region where the ngrok client will connect to host its tunnels.

| ----- |
| `us` |

default

| United States |
| `eu` | | Europe |
| `ap` | | Asia/Pacific |
| `au` | | Australia |
| `sa` | | South America |
| `jp` | | Japan |
| `in` | | India |

---

## `root_cas`

The root certificate authorities used to validate the TLS connection to the ngrok server.

| ----- |
| `trusted` |

default

| use only the trusted certificate root for the ngrok.com tunnel service |
| `host` | | use the root certificates trusted by the host's operating system. You will likely want to use this option to connect to third-party ngrok servers. |
| other values | | path to a certificate PEM file on disk with certificate authorities to trust |

---

## `socks5_proxy`

URL of a SOCKS5 proxy to use for establishing a connection to the ngrok server.

    socks5_proxy: "socks5://localhost:9150"

---

## `tunnels`

A map of names to tunnel definitions. See [Tunnel definitions][12] for more details.

---

## `update`

| ----- |
| `true` | | automatically update ngrok to the latest version, when available |
| `false` |

default

| never update ngrok unless manually initiated by the user |

---

## `update_channel`

The update channel determines the stability of released builds to update to. Use 'stable' for all production deployments.

| ----- |
| `stable` |

default

| channel |
| `beta` | | update to new beta builds when available |

---

## `web_addr`

Network address to bind on for serving the local web interface and api.

| ----- |
| network address | | bind to this network address |
| `127.0.0.1:4040` |

default

| default network address |
| `false` | | disable the web UI |
