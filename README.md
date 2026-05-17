# claudeclaw-plus-container-unraid

<p align="center">
  <img src="icons/claudeclaw-plus-container.png" alt="claudeclaw-plus" width="200" />
</p>

[![Lint](https://github.com/paulmeier/claudeclaw-plus-container-unraid/actions/workflows/lint.yml/badge.svg)](https://github.com/paulmeier/claudeclaw-plus-container-unraid/actions/workflows/lint.yml)
[![Release Please](https://github.com/paulmeier/claudeclaw-plus-container-unraid/actions/workflows/release-please.yml/badge.svg)](https://github.com/paulmeier/claudeclaw-plus-container-unraid/actions/workflows/release-please.yml)

Unraid Community Applications template for [claudeclaw-plus-container](https://github.com/paulmeier/claudeclaw-plus-container) — a Docker container running [ClaudeClaw+](https://github.com/TerrysPOV/ClaudeClaw-Plus) (a superset of vanilla [claudeclaw](https://github.com/moazbuilds/claudeclaw) with added governance, durable multi-step orchestration, persistent cross-session memory, and a hardened web UI) as a persistent Claude Code personal assistant daemon.

> Looking for the vanilla template? See [paulmeier/claudeclaw-container-unraid](https://github.com/paulmeier/claudeclaw-container-unraid). Both can be installed and run side-by-side on the same Unraid server.

## Template

- **Container image**: `ghcr.io/paulmeier/claudeclaw-plus-container:latest`
- **Project**: https://github.com/paulmeier/claudeclaw-plus-container
- **Container registry**: https://github.com/paulmeier/claudeclaw-plus-container/pkgs/container/claudeclaw-plus-container

## Installation

### Via Community Applications (recommended)

Search for **claudeclaw-plus** in the Unraid Apps tab.

To add this repository as a template source manually:

1. In Unraid, open the **Apps** tab and go to **Settings**.
2. Under **Template Repositories**, add:
   ```
   https://github.com/paulmeier/claudeclaw-plus-container-unraid
   ```
3. Click **Check for Updates**, then search for **claudeclaw-plus**.

### Manual installation

1. In Unraid, go to **Docker** → **Add Container**.
2. Click **Load template from URL** and paste:
   ```
   https://raw.githubusercontent.com/paulmeier/claudeclaw-plus-container-unraid/main/templates/claudeclaw-plus-container.xml
   ```
3. Click **Load**, fill in any required fields, and click **Apply**.

## First-run authentication

ClaudeClaw+ uses your Claude Code subscription — no API key needed. After the container starts for the first time, open a terminal to it and run:

```bash
claude login
```

This opens an OAuth browser flow. Complete it once and credentials are saved permanently in the app data volume.

## Configuration

Edit `settings.json` in your app data directory (`/mnt/user/appdata/claudeclaw-plus/claudeclaw/settings.json`) to configure messaging bridges and other options. ClaudeClaw+ inherits the upstream `claudeclaw` settings schema and adds its own keys for governance, model routing, orchestration, and persistent memory — see [the ClaudeClaw+ docs](https://github.com/TerrysPOV/ClaudeClaw-Plus) for the Plus-specific reference.

## Running side-by-side with vanilla `claudeclaw`

Both containers can be installed on the same Unraid box. They use distinct container names (`claudeclaw` vs `claudeclaw-plus`), distinct default appdata paths (`/mnt/user/appdata/claudeclaw` vs `/mnt/user/appdata/claudeclaw-plus`), and each gets its own web dashboard. If you want them on the same host port externally, change one of the Web Dashboard Port mappings.

## Tailscale

The template supports Unraid's Community Applications Tailscale integration. To enable it:

1. Open the claudeclaw-plus container in **Docker** and switch to **Advanced View**.
2. Toggle **Use Tailscale** on and fill in the standard Tailscale fields (hostname, exit node, etc.).
3. Apply.

The template pre-declares `CA_TS_FALLBACK_DIR=/root/.claude/tailscale`, so Tailscale's state (machine key, node info) lands inside the persistent appdata volume at `/mnt/user/appdata/claudeclaw-plus/tailscale/` and survives container recreation and image updates. Without this, Unraid's Tailscale hook errors with `Couldn't detect persistent Docker directory for .tailscale_state!` because it can't auto-recognize `/root/.claude` as a config path.

## Support

Open an issue: https://github.com/paulmeier/claudeclaw-plus-container-unraid/issues
