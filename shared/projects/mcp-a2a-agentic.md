# Agentic AI Module — MCP & A2A with Kong Gateway

> Module: `mcp-a2a/` | 6 incremental decK files | MCP backend + Keycloak in Docker

## Steps

| Step | File | Feature | Auth |
|------|------|---------|------|
| 1 | `01-mcp-passthrough.yaml` | MCP Passthrough Listener (native MCP proxy) | None |
| 2 | `02-passthrough-auth.yaml` | + Key-Auth + Rate-Limiting on MCP | key-auth |
| 3 | `03-conversion-listener.yaml` | REST → MCP conversion (httpbin tools) | None |
| 4 | `04-aggregation.yaml` | Multi-team tool aggregation | None |
| 5 | `05-mcp-oauth2.yaml` | OAuth2 PKCE with Keycloak | OAuth2 PKCE |
| 6 | `06-a2a-routing.yaml` | A2A agent routing + per-agent auth | key-auth per agent |

## MCP Plugin Modes

| Mode | Client → Kong | Kong → Upstream | Use Case |
|------|--------------|-----------------|----------|
| `passthrough-listener` | MCP JSON-RPC | MCP JSON-RPC (unchanged) | MCP-native backend |
| `conversion-listener` | MCP JSON-RPC | REST HTTP calls | Existing REST APIs |
| `conversion-only` | — (no listener) | — (registers tools) | Per-team tool definitions |
| `listener` | MCP JSON-RPC | Aggregates tagged conversion-only tools | Unified multi-team endpoint |

## Decision Tree

```
Is your upstream already an MCP server?
  ├── YES → passthrough-listener
  └── NO (REST API)
        ├── Single backend? → conversion-listener
        └── Multiple backends? → conversion-only (per team) + listener (aggregate)
```

## ai-mcp-oauth2 Plugin Notes
- Uses MCP OAuth 2.0 spec (NOT generic OIDC)
- Required: `resource` (MCP endpoint URL), `authorization_servers` (must match token `iss`)
- `authorization_servers` MUST match token's `iss` claim exactly
- `jwks_endpoint` should use Docker hostname (reachable from Kong)
- `insecure_relaxed_audience_validation: true` needed for Keycloak
- MUST NOT pair with conversion-listener mode

## conversion-listener Key Details
- Tool `path` values are Kong internal routes (subrequests), not direct upstream paths
- Client must call `initialize` first to get `mcp-session-id` header
- All subsequent calls need `mcp-session-id` header
- Response format: `text/event-stream` with `event: message\ndata: {...}`

## A2A Routing (Step 6)
- Agent Card served at `GET /.well-known/agent.json`
- Per-agent routes: `/a2a/flights`, `/a2a/hotels`, `/a2a/weather`
- Each agent can have independent auth and rate limits

## Docker Services

| Service | Port | Purpose |
|---------|------|---------|
| MCP Backend | 3001 | Travel tools: flights, hotels, weather |
| Keycloak | 8080 | OIDC provider for OAuth2 PKCE (Step 5) |

## Docker Networking Gotcha
- Kong DP must be connected to `mcp-a2a_kong-net` to resolve container hostnames
- May need `dns_resolver = 127.0.0.11` in Kong config
