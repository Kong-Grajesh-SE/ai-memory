# AI Gateway Module — Enterprise LLM Controls

> Module: `ai-gateway/` | 10 incremental decK files | Docker services for PII & compression

## Plugin Chain (Execution Order)

```
Request → ai-prompt-template → ai-prompt-decorator → ai-prompt-guard
        → ai-semantic-prompt-guard → ai-sanitizer → ai-semantic-cache
        → ai-prompt-compressor → ai-rate-limiting-advanced
        → ai-proxy-advanced (round-robin: Mistral ↔ Cerebras) → LLM
        → ai-semantic-response-guard → ai-sanitizer (response) → Client
```

## Steps

| Step | File | Plugin | Purpose |
|------|------|--------|---------|
| 01 | `01-ai-proxy-advanced.yaml` | ai-proxy-advanced | Multi-provider LB (Mistral + Cerebras round-robin) |
| 02 | `02-prompt-decorator.yaml` | ai-prompt-decorator | System prompt injection |
| 03 | `03-prompt-guard.yaml` | ai-prompt-guard | Regex-based prompt injection blocking |
| 04 | `04-semantic-cache.yaml` | ai-semantic-cache | Redis vector cache for similar queries |
| 05 | `05-prompt-template.yaml` | ai-prompt-template | Named reusable prompt templates |
| 06 | `06-ai-sanitizer.yaml` | ai-sanitizer | PII redaction (requests + responses) |
| 07 | `07-prompt-compressor.yaml` | ai-prompt-compressor | Token compression for cost reduction |
| 08 | `08-ai-rate-limiting.yaml` | ai-rate-limiting-advanced | Token-aware rate limits per model |
| 09 | `09-semantic-prompt-guard.yaml` | ai-semantic-prompt-guard | Embedding-based prompt content filtering |
| 10 | `10-semantic-response-guard.yaml` | ai-semantic-response-guard | Embedding-based response content filtering |

## AI Providers

| Provider | Model | Role |
|----------|-------|------|
| Mistral | `mistral-tiny` | Primary chat model |
| Cerebras | `gpt-oss-120b` | Secondary chat model (round-robin) |
| Mistral | `mistral-embed` | Embeddings (semantic cache, guards) |

## Supporting Docker Services

| Service | Port | Used From | Purpose |
|---------|------|-----------|---------|
| Redis Stack | 6379 | Step 4+ | Semantic cache, rate limiting, semantic guards |
| AI PII Service | 8086 | Step 6+ | PII anonymization (NLP-based) |
| AI Compress Service | 8085 | Step 7+ | Prompt token compression |

## Key Environment Variables

```bash
export DECK_MISTRAL_API_KEY="<mistral-key>"
export DECK_CEREBRAS_API_KEY="<cerebras-key>"
```

## Key Learnings
- `deck file add-plugins` merges plugin-only files into live state (Steps 2–10)
- Only Step 1 deploys the service/route; subsequent steps add plugins incrementally
- Docker services use `host.docker.internal` to reach from Kong DP
- Semantic features (cache, guards) require Redis Stack with vector support
