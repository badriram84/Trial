# Snowflake + dbt POC (Medallion Architecture)

This repo contains a reproducible Snowflake dev setup (Stage 0) and will evolve into:
- Bronze/Silver/Gold transformations using dbt
- Data quality checks
- Provider ACCESS Readiness Index POC
- Claims Digital Twin POC extensions

## Quick Start
1. Run `infra/00_bootstrap.sql`
2. Run `infra/01_grants.sql`
3. Follow `docs/runbook.md` for validation queries and role assignments.
