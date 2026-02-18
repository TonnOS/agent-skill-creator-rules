---
name: {{SKILL-NAME}}
description: |
  Interact with {{API_NAME}} for {{FUNCTIONALITY}}. Use when:
  (1) {{USE_CASE_1}}
  (2) {{USE_CASE_2}}
  (3) {{USE_CASE_3}}
metadata:
 {
 "openclaw": {
   "requires": { "env": ["{{API_KEY_ENV}}"] },
   "primaryEnv": "{{API_KEY_ENV}}"
 }
 }
---

# {{SKILL-NAME}}

{{BRIEF_OVERVIEW}}

## Authentication

API key is automatically loaded from `{{API_KEY_ENV}}` environment variable.

## Endpoints

### GET {{ENDPOINT_1}}

```bash
curl -H "Authorization: Bearer ${{API_KEY_ENV}}" \
  {{API_BASE_URL}}/{{ENDPOINT_1}}
```

### POST {{ENDPOINT_2}}

```bash
curl -X POST \
  -H "Authorization: Bearer ${{API_KEY_ENV}}" \
  -H "Content-Type: application/json" \
  -d '{{REQUEST_BODY_EXAMPLE}}' \
  {{API_BASE_URL}}/{{ENDPOINT_2}}
```

## Response Format

```json
{
  "success": true,
  "data": { }
}
```

## Error Handling

| Status Code | Meaning | Action |
|-------------|---------|--------|
| 401 | Unauthorized | Check API key |
| 429 | Rate limited | Wait and retry |
| 500 | Server error | Retry later |

## Advanced Topics

- **Pagination**: See [references/pagination.md](references/pagination.md)
- **Webhooks**: See [references/webhooks.md](references/webhooks.md)
