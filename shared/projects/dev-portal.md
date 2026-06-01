# Developer Portal Module — Bookstore API

> Module: `api-portal/` | OpenAPI-first workflow | Konnect UI + API instructions

## The Flow

```
OpenAPI Spec → deck file openapi2kong → deck file add-plugins (CORS)
→ deck gateway sync → Create API Product → Publish to Portal
→ Developer registers → Creates app → Gets API key → Calls API
```

## Steps

| Step | Action | Tool |
|------|--------|------|
| 1 | Convert OpenAPI to Kong config | `deck file openapi2kong` |
| 2 | Add CORS plugin (required for "Try It") | `deck file add-plugins` |
| 3 | Validate config | `deck file validate` + `deck gateway validate` |
| 4 | Preview changes | `deck gateway diff` |
| 5 | Deploy to gateway | `deck gateway sync` |
| 6 | Create Developer Portal | Konnect UI or API |
| 7 | Create API Product | Konnect UI or API |
| 8 | Upload OpenAPI spec as version | Konnect UI or API |
| 9 | Link to gateway service (implementation) | Konnect UI or API |
| 10 | Add portal pages (getting-started, ToS, changelog) | Konnect UI or API |
| 11 | Add API documentation | Konnect UI or API |
| 12 | Attach auth strategy (key-auth) | Konnect UI or API |
| 13 | Publish to portal | Konnect UI or API |
| 14 | Developer self-service flow | Portal UI |

## Key Concepts

- **DO NOT** add `key-auth` plugin via decK when using portal auth strategies
- Konnect auto-creates `konnect-application-auth` plugin when you attach an auth strategy
- Adding manual `key-auth` via decK conflicts with portal-issued keys
- CORS plugin is required for the Dev Portal's "Try It" feature

## Portal Configuration

```json
{
  "authentication_enabled": true,
  "auto_approve_developers": false,
  "auto_approve_applications": false,
  "default_api_visibility": "public",
  "default_page_visibility": "public"
}
```

## Konnect API Base URLs

| Region | URL |
|--------|-----|
| US | `https://us.api.konghq.com` |
| EU | `https://eu.api.konghq.com` |
| AU | `https://au.api.konghq.com` |

## File Structure

```
api-portal/
├── openapi/bookstore-api.yaml    ← OpenAPI spec
├── deck/plugin-cors.yaml         ← CORS plugin for add-plugins
├── pages/                        ← Portal page content (getting-started, ToS, changelog)
└── docs/books-quickstart.md      ← API-level documentation
```
