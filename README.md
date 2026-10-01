# Whatsapp-MCP-Docker

Docker images for [verygoodplugins/whatsapp-mcp](https://github.com/verygoodplugins/whatsapp-mcp). This repo contains only Dockerfiles and a build workflow. The upstream code is not forked.

| Image | Source |
|---|---|
| `ghcr.io/jaytalge/whatsapp-bridge:<tag>` / `:latest` | `whatsapp-bridge/` (Go, whatsmeow) |
| `ghcr.io/jaytalge/whatsapp-mcp:<tag>` / `:latest` | `whatsapp-mcp-server/` (Python, MCP over streamable HTTP) |

## Build

- Every day at 03:17 UTC the workflow checks for the latest upstream **release tag**. It builds both images only if that tag is not in GHCR yet. It never builds `main`.
- To build manually, use Actions → "Build images from upstream release" → Run workflow. You can enter a specific tag and choose "force".
- A push to `bridge/`, `mcp/` or the workflow rebuilds the current release.

## Runtime notes

- The bridge only listens on `127.0.0.1:8080` and only accepts loopback Host headers. The MCP server therefore has to share its network namespace (`network_mode: service:bridge`).
- Both containers mount the same volume at `/app/store`. It holds the WhatsApp session, `messages.db`, media and `.bridge-token`.
- Set the same `WHATSAPP_BRIDGE_TOKEN` (at least 16 characters) for the bridge and the MCP server.
- Outbound webhooks (`WEBHOOK_URL`) send the bridge token in the `X-Bridge-Token` header.
