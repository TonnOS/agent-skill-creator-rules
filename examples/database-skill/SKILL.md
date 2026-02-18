---
name: database-queries
description: |
  Common database queries and operations for analytics. Use when:
  (1) Running analytics queries
  (2) Generating reports
  (3) Data aggregation tasks
---

# Database Queries

Pre-built queries for common analytics tasks.

## Connection

Database URL is loaded from `DATABASE_URL` environment variable.

## Quick Queries

### Daily Active Users

```sql
SELECT DATE(created_at) as date, COUNT(DISTINCT user_id) as users
FROM sessions
WHERE created_at >= NOW() - INTERVAL '30 days'
GROUP BY DATE(created_at)
ORDER BY date DESC;
```

### Revenue by Month

```sql
SELECT 
  DATE_TRUNC('month', created_at) as month,
  SUM(amount) as revenue
FROM payments
WHERE status = 'completed'
GROUP BY DATE_TRUNC('month', created_at)
ORDER BY month DESC;
```

## Query Scripts

For complex queries, use the bundled scripts:

```bash
# Run analytics report
python3 {baseDir}/scripts/run_report.py --report daily_metrics

# Export data
python3 {baseDir}/scripts/export.py --table users --format csv
```

## References

- **Schema documentation**: See [references/schema.md](references/schema.md)
- **Query patterns**: See [references/patterns.md](references/patterns.md)
- **Performance tips**: See [references/performance.md](references/performance.md)
