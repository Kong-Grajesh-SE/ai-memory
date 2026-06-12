# Kong Konnect Bootcamp — Project Notes

> Repo: [Kong-Grajesh-SE/bootcamp-repo](https://github.com/Kong-Grajesh-SE/)
> Last updated: June 12, 2026

Hands-on labs for Kong Konnect — from API gateway fundamentals to AI-powered agentic workflows. Each module is self-contained with declarative decK files, Insomnia collections, and step-by-step walkthroughs.

## Modules Covered

| Module | Directory | Status |
|--------|-----------|--------|
| API Gateway (14 plugins) | `api-gateway/` | ✅ Complete (with diagrams & recaps) |
| AI Gateway (10 plugins) | `ai-gateway/` | ✅ Complete (with diagrams & recaps) |
| Agentic AI — MCP & A2A | `mcp-a2a/` | ✅ Complete (with diagrams & recaps) |
| APIOps (decK mastery) | `apiops/` | ✅ Complete (with diagrams & recaps) |
| Developer Portal | `api-portal/` | ✅ Complete (with diagrams & recaps) |
| Event Gateway | `event-gateway/` | ✅ Complete (with diagrams & recaps) |
| Metering & Billing | `metering-billing/` | ✅ Complete (with diagrams & recaps) |
| Context Mesh | `context-mesh/` | ✅ Complete (with diagrams & recaps) |
| Bring Your Own Agent | `byoa/` | ✅ Complete (with diagrams & recaps) |

## Recommended Learning Order

1. **api-gateway** — Core Kong concepts (services, routes, plugins, consumers)
2. **apiops** — decK workflows (sync, diff, dump, lint, tags, templates)
3. **ai-gateway** — AI/LLM controls on top of gateway fundamentals
4. **mcp-a2a** — Agentic AI patterns (MCP, A2A) with Kong
5. **api-portal** — Publish and manage APIs through the Developer Portal

## Shared Prerequisites

- Kong Konnect account with a control plane
- decK CLI installed (`brew install kong/deck/deck`)
- Docker Desktop for local services
- Konnect Personal Access Token (PAT)
- curl + jq (pre-installed on macOS)
- Insomnia for GUI API testing

## Key Environment Variables

```bash
export KONNECT_TOKEN="<your-konnect-pat>"
export CP_NAME="<your-control-plane-name>"
export PROXY_URL=http://localhost:8000   # or serverless gateway URL
```
