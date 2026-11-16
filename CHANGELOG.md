# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Upstream image tracks PairDrop v1.11.2 (LinuxServer.io build `ls132`) as defined in `service.yml`.

## [0.0.1]

### Added

- Initial App-as-Code service definition for PairDrop on port 3000 with Traefik routing and automatic DNS.
- Environment settings for rate limiting, WebSocket fallback and debug mode.
- README with configuration reference and project icon.
