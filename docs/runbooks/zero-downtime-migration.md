# Zero/low-downtime migration playbook (practical)

Goal: make schema changes safe and boring by using a repeatable sequence:
preflight → deploy-compatible schema change → backfill → verify → optional constraints/indexes.

This repository includes an executable example:
- `scripts/migrate_safe_add_column.sh`

## Drill steps
```bash
make up
make seed
make migrate
```

## Why this pattern is safer
- Adding a nullable column is typically fast and low risk.
- Backfilling in batches reduces lock time and avoids long-running transactions.
- Creating indexes concurrently reduces lock impact (but can take longer).
- Verification is explicit and automated.

## Production notes
- Always preflight: table size, expected lock levels, and maintenance windows.
- Have a backout plan (feature flags, rollback scripts, or compensating migrations).
- Monitor: replication lag, active locks, and query latency during the migration.
