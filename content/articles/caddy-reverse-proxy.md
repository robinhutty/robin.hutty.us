---
title: Reverse Proxying with Caddy
date: 2025-10-15
modified: 2026-02-13T15:53:57
categories: services
tags: http,oci,container,proxy,caddy
type: docs
draft: true
---

I use [Caddy][1] as a reverse proxy in front of several applications that I run for personal/fam usage. This doc shows some of how I have it set up.

[1]: https://caddyserver.com

## Caddy

### `Caddyfile`

```config
{
  # Global Options: https://caddyserver.com/docs/caddyfile/options

  email robin@example.com
  # Set a default TLS ServerName for when clients do not use SNI in their ClientHello
  default_sni example.com

  # In case I want to _disable_ Caddy's automagic for HTTPS: https://caddyserver.com/docs/automatic-https
  # auto_https off

  metrics {
    per_host
  }

}

# Server Options: https://caddyserver.com/docs/caddyfile/options#server-options
servers {
    # https://caddyserver.com/docs/caddyfile/options#enable-full-duplex
    # enable_full_duplex
}

http://hello {
  respond "Hi"
}

http://homepage {
  reverse_proxy homepage:3000
}

# For HTTPS, the first line would be:
# ntfy.example.com, ntfy {

http://ntfy.example.com, http://ntfy {
  encode zstd gzip
  reverse_proxy ntfy:80 {
  }
}

vcs {
  reverse_proxy forge:3000 {  # TODO: correct the port number(s)
    header_up X-Real-Ip {remote_host}
  }
}
```


## containerd/nerdctl compose

This assumes a `containerd` setup as I have described in my [article about container runtimes](../container-runtimes/), especially `nerdctl compose ...`.

### `./.env`

A `.env` file [sets values for variables](https://docs.docker.com/compose/how-tos/environment-variables/envvars/) used in the compose file(s).

```env
# https://docs.docker.com/compose/how-tos/environment-variables/envvars/
COMPOSE_PROJECT_NAME=example
DEFAULT_TAG=latest
PUID=1000
PGID=1000
CADDY_TAG=2.10.2
```

### `compose.yaml`

```yaml
# See https://docs.docker.com/reference/compose-file/
# See https://docs.docker.com/compose/how-tos/
# See https://docs.docker.com/reference/compose-file/include/
# include:
  # - path: ./foo/compose.yaml

# # ref: https://hub.docker.com/_/caddy
networks:
  caddy-net:
    driver: bridge
    name: caddy-net

volumes:
  # # ref: https://hub.docker.com/_/caddy
  caddy_data:
    # external: true # May need to create volume with 'docker volume create caddy_data'
  caddy_config:

services:
  caddy:
    container_name: caddy
    image: caddy:${CADDY_TAG:-${DEFAULT_TAG}}
    networks:
      - caddy-net # Network exclusively for Caddy-proxied containers
    restart: unless-stopped
    # annotations:
    #   nerdctl/bypass4netns: true

    networks:
      - caddy-net
    ports:
      - "80:80"
      - "8443:443"
      - "8443:443/udp" # QUIC protocol support: https://www.chromium.org/quic
    environment:
      - TZ=America/New_York
    volumes:
      - ./caddy/conf:/etc/caddy # must contain the Caddyfile (or JSON-based Caddy config: https://caddyserver.com/docs/json)
      - ./caddy/Caddyfile:/etc/caddy/Caddyfile # config file on host for easy editing.
      # - ./foo:/srv  # Use this if you want Caddy to serve a static website from the host filesystem and not just using Caddy as a reverse proxy
      - caddy_config:/config
    cap_add:
      - NET_ADMIN

  homepage:
    container_name: homepage
    image: ghcr.io/gethomepage/homepage:${HOMEPAGE_TAG:-${DEFAULT_TAG}}
    environment:
      # HOMEPAGE_ALLOWED_HOSTS is required, may need port.
      HOMEPAGE_ALLOWED_HOSTS: home.example.com # or set to '*', but see https://gethomepage.dev/installation/#homepage_allowed_hosts
      # The PUID/PGID variables are sourced from a .env and provide the UID/GID in order to run as non-root. This might require set up of [containerd rootless](https://github.com/containerd/nerdctl/blob/main/docs/rootless.md) or [rootless mode for dockerd/moby](https://rootlesscontaine.rs/getting-started/docker/)
      # The UID must be part of the 'docker' group in order to use docker integrations
      PUID: $PUID
      PGID: $PGID

    # annotations:
    #   nerdctl/bypass4netns: true  # if running under containerd including via colima

    logging:
      driver: "json-file"
      options:
        max-size: "500k"
        max-file: "1"

    networks:
      - caddy-net

    ports:
      - "3000:3000"
    volumes:
      - ./homepage/config:/app/config # Make sure your local config directory exists
      - /var/run/docker.sock:/var/run/docker.sock:ro # optional, for docker integrations
    restart: unless-stopped


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


  # forge:
  #   container_name: forgejo
  #   image: codeberg.org/forgejo/forgejo:${FORGEJO_TAG:-${DEFAULT_TAG}}
  # <snip>
```

## Testing

1. Trivial testing - does it insta-fail, does it run?

    ```console
    $ nerdctl compose config
    <snip>  # should output a fully resolved configuration for nerdctl compose
    $ nerdctl compose up -d # should start all compose servicese
    INFO[0000] Ensuring image ghcr.io/gethomepage/homepage:latest
    INFO[0000] Ensuring image caddy:2.10.2
    INFO[0000] Creating container homepage
    INFO[0000] Running [/usr/local/bin/nerdctl run <snip>
    INFO[0001] Creating container caddy
    INFO[0001] Running [/usr/local/bin/nerdctl run <snip>
    $ nerdctl compose ps
    NAME        IMAGE                                  COMMAND                   SERVICE     STATUS     PORTS
    caddy       docker.io/library/caddy:2.10.2         "caddy run --config …"    caddy       running    0.0.0.0:8080->80, 8443->443/tcp, 0.0.0.0:8443->443/udp
    homepage    ghcr.io/gethomepage/homepage:latest    "docker-entrypoint.s…"    homepage    running    0.0.0.0:3000->3000/tcp
    ```

2. Does the simplest Virtual Host return a good response?

    ```console
    $ curl --header 'Host:hello' http://localhost:8080
    Hi
    ```

3. Do all Services do the thing?

## Notes

> If you're using Caddy to reverse proxy to another container, remember that in Docker networking, localhost means "this container", not "this machine". So for example, do not use `reverse_proxy localhost:8080`, instead use `reverse_proxy other-container:8080`

* logging: https://caddyserver.com/docs/logging
* TODO: compare with https://willschenk.com/howto/2023/using_caddy_docker_proxy/
