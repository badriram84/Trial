# Snowflake + dbt POC Runbook (Stage 0: Platform Bootstrap)

## Purpose
This repo bootstraps a reproducible Snowflake dev environment for POCs using a Medallion layout:
- BRONZE (raw landing)
- SILVER (conformed/clean)
- GOLD (serving/marts)

It also sets up minimal RBAC:
- ROLE_DBT_DEV: build/transformation role (for dbt)
- ROLE_POC_READONLY: consumer read access to GOLD

---

## Prerequisites
- Snowflake trial account (AWS India/Mumbai is fine)
- Ability to run SQL as a high-privilege role (ACCOUNTADMIN in trial)
- Snowflake user: `<YOUR_SNOWFLAKE_USER>`
- Repo cloned locally

---

## Conventions
### Objects
- Warehouse: `WH_DEV`
- Database: `POC_DEV`
- Schemas: `POC_DEV.BRONZE`, `POC_DEV.SILVER`, `POC_DEV.GOLD`
- Roles: `ROLE_DBT_DEV`, `ROLE_POC_READONLY`

### Medallion meaning
- BRONZE: raw tables or minimally standardized views + ingestion metadata
- SILVER: deduped/standardized conformed tables/views
- GOLD: marts/scorecards for consumption and dashboards

---

## Step 0 — Bootstrap (Run These SQL Files)
Run these from Snowflake Worksheet in this order:

1) `infra/00_bootstrap.sql`
   - creates WH_DEV, POC_DEV, schemas, roles

2) `infra/01_grants.sql`
   - grants for ROLE_DBT_DEV (warehouse/database/schema usage + create privileges)
   - grants for ROLE_POC_READONLY (usage + select on ALL/FUTURE tables/views in GOLD)

> Note: In trial accounts, use `ACCOUNTADMIN` for setup.

---

## Step 1 — Assign Roles to Your User
Run the following (edit username):

```sql
USE ROLE ACCOUNTADMIN;
GRANT ROLE ROLE_DBT_DEV TO USER <YOUR_SNOWFLAKE_USER>;
GRANT ROLE ROLE_POC_READONLY TO USER <YOUR_SNOWFLAKE_USER>;
