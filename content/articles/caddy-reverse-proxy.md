---
title: Reverse Proxying with Caddy
date: 2025-10-15
modified: 2025-10-15T18:46:24
categories: services
tags: cicd
type: docs
draft: true
---

I use [Caddy][1] as a reverse proxy in front of several applications that I run for personal/fam usage. This doc shows some of how I have it all set up.

[1]: https://caddyserver.com

## Caddy

```config
{
# Global Options: https://caddyserver.com/docs/caddyfile/options

  email me@example.com
  # Set a default TLS ServerName for when clients do not use SNI in their ClientHello
  default_sni example.com

  # In case I want to _disable_ Caddy's automagic for HTTPS: https://caddyserver.com/docs/automatic-https
  # auto_https off

  metrics {
    per_host
  }

}

# For not-HTTPS, the first line would be:
# http://ntfy.example.com, http://ntfy {

ntfy.example.com, ntfy {
  encode zstd gzip
  reverse_proxy ntfy:80 {
  }
}
```


## docker compose

```yaml
services:
  # Caddy example derived from Caddy's own example at https://hub.docker.com/_/caddy
  caddy:
    container_name: caddy
    image: caddy:latest
    networks:
      - caddy-net # Network exclusively for Caddy-proxied containers
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp" # QUIC protocol support: https://www.chromium.org/quic
    environment:
      - TZ=America/New_York
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile # config file on host in same directory as docker-compose.yml for easy editing.
      # - ./foo:/srv  # Use this if you want Caddy to serve a static website from the host filesystem and not just using Caddy as a reverse proxy
      - caddy_config:/config
    cap_add:
      - NET_ADMIN

  ntfy:
    container_name: ntfy
    image: binwiederhier/ntfy
    networks:
      - caddy-net
    restart: unless-stopped
    ports:
      - "8081:80"
    logging:
      driver: "json-file"
      options:
        max-size: "500k"
        max-file: "1"
    command: serve
    environment:
      - TZ=America/New_York
    user: 1000:1000
    volumes:
      - ./ntfy/cache:/var/cache/ntfy
      - ./ntfy/config:/etc/ntfy
```
