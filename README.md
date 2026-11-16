<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="PairDrop logo" width="128">
  </picture>
</p>

<h1 align="center">aac-pairdrop</h1>

<p align="center">Self-hosted deployment definition for PairDrop, a browser-based AirDrop alternative for the local network.</p>

<p align="center">
  <a href="https://github.com/schlagmichdoch/PairDrop"><img alt="Upstream" src="https://img.shields.io/badge/upstream-schlagmichdoch%2FPairDrop-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-lscr.io%2Flinuxserver%2Fpairdrop%3Av1.11.2--ls132-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/schlagmichdoch/PairDrop/blob/master/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-GPL--3.0-green"></a>
</p>

## About

PairDrop is a peer-to-peer file and text sharing tool that works in the browser on all
platforms. Devices in the same local network (or paired devices) exchange images, documents
and text directly over a WebRTC connection, so the content does not need to be stored on the
server. No account is required to send or receive.

Key features (as provided by upstream):

- Peer-to-peer transfer of files and text between devices in the same network
- Pairing of devices for transfers across different networks
- Works in any modern browser on desktop and mobile
- Optional WebSocket fallback when a direct WebRTC connection is not possible
- Optional rate limiting and custom STUN/TURN server configuration

## Upstream

- Project: <https://github.com/schlagmichdoch/PairDrop>
- Container image: `lscr.io/linuxserver/pairdrop:v1.11.2-ls132` (LinuxServer.io build)
- License: GPL-3.0 (upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, the external networks used for routing must exist first:

```bash
docker network create services-secured
docker network create services-exposed
docker network create docker-default

cd deployments/docker_compose
docker compose up -d
```

This deployment needs no secrets. The `.env` file holds only non-sensitive values.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `TZ` | `Europe/Berlin` | Time zone |
| `PUID` | `1000` | User ID the container runs as |
| `PGID` | `1000` | Group ID the container runs as |
| `RATE_LIMIT` | `false` | Limit clients to 100 requests per 5 minutes when `true` |
| `WS_FALLBACK` | `false` | Route transfers through the server via WebSocket when WebRTC is unavailable (all traffic is then readable by the server) |
| `DEBUG_MODE` | `false` | Log client IP addresses for debugging the HTTP setup. Do not use in production |
| `SERVICE_NAME` | `aac-pairdrop` | Service name |
| `SERVICE_HOSTNAME` | `share` | Container hostname |
| `SERVICE_DOMAIN_NAME` | `CHANGE_ME` | Domain used for the public URL of the instance |
| `SERVICE_IMAGE_NAME` | `CHANGE_ME` | Container image name (rendered from `service.yml`) |
| `SERVICE_IMAGE_TAG` | `CHANGE_ME` | Container image tag (rendered from `service.yml`) |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `3000` | TCP | Web interface (`web`), published as `23200` on the host |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/config` | Application configuration files |

Running behind a reverse proxy over HTTPS is recommended. The proxy must set the
`X-Forwarded-For` header correctly, otherwise all clients appear as the same peer.

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-pairdrop>.

## Credits and license

PairDrop is developed by its upstream project and licensed under GPL-3.0. This repository
only contains the deployment definition for it and is not a fork of the application. The
logo is provided through the selfh.st icon collection; see
[`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources and licenses.
