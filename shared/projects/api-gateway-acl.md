# API Gateway Module — Plugins & ACL

> Module: `api-gateway/` | 14 incremental decK files | Insomnia collection included

## Plugin Chain (14 Steps)

| Step | File | Plugin | Purpose |
|------|------|--------|---------|
| 01 | `01-services-and-routes.yaml` | — | Base service + route (httpbin) |
| 02 | `02-rate-limiting.yaml` | rate-limiting | Throttle requests |
| 03 | `03-proxy-cache.yaml` | proxy-cache | Cache responses |
| 04 | `04-upstream.yaml` | — | Upstream + targets (load balancing) |
| 05 | `05-key-auth.yaml` | key-auth | API key authentication |
| 06 | `06-jwt-auth.yaml` | jwt | JWT token authentication |
| 07 | `07-consumers.yaml` | — | Consumer entities |
| 08 | `08-cors.yaml` | cors | Cross-origin resource sharing |
| 09 | `09-ip-restriction.yaml` | ip-restriction | Allow/deny by IP |
| 10 | `10-correlation-id.yaml` | correlation-id | Request tracing headers |
| 11 | `11-request-transformer.yaml` | request-transformer | Modify upstream requests |
| 12 | `12-response-transformer.yaml` | response-transformer | Modify downstream responses |
| 13 | `13-http-log.yaml` | http-log | Send logs to external endpoint |
| 14 | `14-consumer-groups-acl.yaml` | key-auth + acl + consumer-groups | Tiered access control |

## ACL + Consumer Groups (Step 14) — Key Concepts

### How ACL Works
- **ACL plugin** restricts access by group membership
- `allow: [premium, standard]` — only consumers in these ACL groups can access
- Consumers NOT in the allow-list get **403 Forbidden**

### Consumer Tiers Configured

| Consumer | ACL Group | Consumer Group | Rate Limit | Access |
|----------|-----------|----------------|------------|--------|
| premium-user | `premium` | `premium-tier` | 1000 req/min | ✅ Allowed |
| standard-user | `standard` | `standard-tier` | 10 req/min | ✅ Allowed |
| trial-user | `trial` | — | — | ❌ Blocked (403) |

### Key Differences: ACL Groups vs Consumer Groups
- **ACL Groups** (`acls`): Authorization — controls *who can access* the API
- **Consumer Groups** (`consumer_groups`): Tiering — controls *rate limits and quotas* per tier
- Both work together: ACL gates access, Consumer Groups differentiate service levels

### Deployment Options
- **Serverless**: Konnect-managed DP, no Docker needed
- **Hybrid**: Docker self-managed DP, full control
- Both use the same decK files and Insomnia collection
