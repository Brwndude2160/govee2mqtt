# Maintained fork

This is a maintained fork of [wez/govee2mqtt](https://github.com/wez/govee2mqtt)
(MIT licensed), created because upstream development has stalled
(last upstream commit 2026-04-08, last release 2026-03-25).

- Branch: `maintained` (default). `main` mirrors upstream as it was when forked.
- Add-on repository URL for Home Assistant: `https://github.com/Brwndude2160/govee2mqtt`
- Images: `ghcr.io/brwndude2160/govee2mqtt` (container) and
  `ghcr.io/brwndude2160/govee2mqtt-{amd64,aarch64}` (Home Assistant add-on)
- A weekly workflow (`.github/workflows/upstream-sync.yml`) opens a PR when
  upstream `main` gains new commits.

## Releasing

1. Set `version:` in `addon/config.yaml` to the new `YYYY.MM.DD-maintained.N`.
2. Commit to `maintained`, then tag that commit with the same name and push the tag.
   The tag build publishes the add-on images under that version.

## Upstream PRs merged

| PR | Description | Notes |
|----|-------------|-------|
| [#720](https://github.com/wez/govee2mqtt/pull/720) | fix(hass): re-assert availability so a missed "online" can't strand entities | clean |
| [#711](https://github.com/wez/govee2mqtt/pull/711) | hass: only re-register on "online", debounce overlapping status triggers | textual conflict with #720 (both added a top-level item at the same spot in `src/service/hass.rs`); kept both |
| [#723](https://github.com/wez/govee2mqtt/pull/723) | iot: recover when libmosquitto stops reconnecting after a TLS EOF | clean |
| [#718](https://github.com/wez/govee2mqtt/pull/718) | fix: show Offline status for devices reported offline | clean |
| [#726](https://github.com/wez/govee2mqtt/pull/726) | feat(lights): honor Home Assistant transition attribute | clean |
| [#703](https://github.com/wez/govee2mqtt/pull/703) | Don't discard successful results when the cache write fails | clean |

## Fork-specific changes

- Image names, add-on `url`, `repository.yaml`, and cosign identity point at this fork.
- Builds run on the `maintained` branch; `:latest` tracks `maintained`.
- Tag builds build the add-on from the container image for that exact tag
  rather than `:latest` (the `GOVEE_IMAGE` build arg).
- Add-on images are built with the composable `home-assistant/builder`
  actions (the legacy `home-assistant/builder` action was deprecated and
  could no longer verify Home Assistant's base-image signatures). The base
  image (`ghcr.io/home-assistant/base-debian:bookworm`) is cosign-verified
  on every build, and published add-on images are signed. `addon/build.yaml`
  is gone; its labels and base image now live in `addon/Dockerfile`.
- A tag build fails if the tag doesn't match `addon/config.yaml`'s version.

## Fork-specific changes

| Version | Change |
|---------|--------|
| 2026.10.23-maintained.2 | Scenes/effects prefer the LAN API (ptReal + Govee's public light-effect library, no login) and fall back to the Platform API only if LAN fails. Fixes effects on lights that are blocked from the internet (Platform API answers "Device is offline"). |
| 2026.10.23-maintained.3 | Security: the HTTP API/UI (unauthenticated, controls every device) is now limited to HA's ingress proxy (172.30.32.2) and loopback by default (`http_allowed_clients` option / `GOVEE_HTTP_ALLOWED_CLIENTS`; `*` restores LAN-wide access). The add-on UI moves to the HA sidebar via ingress; UI asset/API paths made relative. Performance: lights reachable on the LAN are no longer polled via the Platform API (saved API quota; those polls failed for internet-blocked lights). |
