# Nango Self-Hosted on Coolify

Self-hosted [Nango](https://github.com/NangoHQ/nango) (v0.72.6) deployment on Coolify behind Cloudflare Tunnel — with full OAuth integration management, Connect UI, and multi-platform integration guides.

## What's Inside

```
├── README.md                          # This file
├── docs/
│   └── nango-sop.md                   # Complete Standard Operating Procedures
├── config/
│   ├── docker-compose.example.yml     # Example Docker Compose for Coolify
│   └── cloudflare-tunnel.example.json # Example Cloudflare Tunnel config
└── .gitignore
```

## Architecture

```
Browser/Agent ──HTTPS──▶ Cloudflare Tunnel
                              │
              ┌───────────────┼───────────────┐
              ▼                               ▼
     nango.example.com               connect.example.com
     (Dashboard + API)                 (Connect UI SPA)
     Port 3003 → 4450                Port 3009 → 4451
              │                               │
              └───────────┬───────────────────┘
                          ▼
                  Nango Docker Container
                  ├── Node.js Server (3003)
                  ├── serve Connect UI (3009)
                  ├── PostgreSQL 16
                  └── Redis 7.2
```

## Key Features

- **Dual-port deployment**: Dashboard (3003) + Connect UI (3009) on separate subdomains
- **Full OAuth flow**: Dashboard "Connect" button works natively via dedicated Connect UI subdomain
- **API proxy**: Route authenticated API calls through Nango without handling tokens
- **Auto-refresh**: Nango transparently refreshes expired OAuth tokens
- **Multi-platform**: Integration guides for n8n, Antigravity, Claude Code, Codex, OpenMausBot

## Quick Start

See [docs/nango-sop.md](docs/nango-sop.md) for the complete Standard Operating Procedures covering:

1. **Adding OAuth integrations** — Provider setup, credentials, scopes
2. **Creating connections** — Dashboard UI, API, and direct OAuth methods
3. **Using the proxy** — Gmail, Calendar, and any Google API
4. **Managing API keys** — Dev/Prod separation, key architecture
5. **Troubleshooting** — Common issues and fixes
6. **Maintenance** — Restart, redeploy, backup, health checks
7. **Platform integrations** — n8n, Antigravity, Claude Code, Codex, OpenMausBot, custom scripts

## Platform Integration Summary

| Platform | Method | Config |
|----------|--------|--------|
| **n8n** | HTTP Request + Nango Proxy headers | Workflow nodes |
| **Antigravity** | `curl` / `@nangohq/mcp` MCP server | `.env.local` + MCP config |
| **Claude Code** | `@nangohq/mcp` MCP server / `curl` | `.mcp.json` |
| **Codex** | `@nangohq/mcp` MCP server | `.codex/mcp.json` |
| **OpenMausBot** | `fetch()` in bot scripts | Container env vars |
| **Scripts/CLI** | `curl` / Python `NangoClient` | Shell env vars |

## Important Notes

- **API base path is ROOT** (`/integrations`, `/connections`), NOT `/api/v1/`
- **Connect UI requires a separate subdomain** — it cannot be served from the same domain as the dashboard
- **Cloudflare Access should NOT be used** on Nango — it interferes with OAuth callbacks and session cookies
- **`NANGO_SERVER_URL` must be the public HTTPS URL**, never `http://localhost:3003`

## Related

- [Nango Docs](https://docs.nango.dev)
- [Nango GitHub](https://github.com/NangoHQ/nango)
- [Coolify](https://coolify.io)

## License

Documentation and configuration files in this repository are licensed under [MIT](LICENSE).
