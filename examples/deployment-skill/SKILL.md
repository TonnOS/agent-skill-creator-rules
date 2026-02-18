---
name: deployment-workflow
description: |
  Multi-step deployment workflow for web applications. Use when:
  (1) Deploying to production or staging
  (2) Running pre-deployment checks
  (3) Managing deployment rollbacks
---

# Deployment Workflow

Standardized deployment process for web applications.

## Pre-Deployment Checklist

1. Run tests: `npm test`
2. Check linting: `npm run lint`
3. Build production: `npm run build`
4. Verify environment variables

## Deploy Steps

### 1. Stage Changes

```bash
git add .
git commit -m "chore: prepare for deployment"
git push origin main
```

### 2. Deploy

```bash
# Via Coolify
curl -X POST $COOLIFY_WEBHOOK_URL

# Or via CLI
coolify deploy --app myapp --env production
```

### 3. Verify

```bash
curl -s https://myapp.example.com/health | jq .
```

## Rollback

If deployment fails:

```bash
# Revert to previous version
git revert HEAD
git push origin main

# Or trigger rollback webhook
curl -X POST $ROLLBACK_WEBHOOK_URL
```

## Environment Variables

Required for deployment:

| Variable | Purpose |
|----------|---------|
| `COOLIFY_WEBHOOK_URL` | Deployment webhook |
| `DATABASE_URL` | Database connection |

See [references/environments.md](references/environments.md) for all environments.

## Troubleshooting

### Build Fails

Check Node.js version matches production:
```bash
node --version  # Should match CI/CD
```

### Database Migration Fails

Run migrations manually:
```bash
npm run db:migrate
```
