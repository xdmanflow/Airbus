# Workstream 1 · Data Engineering Foundations

**Objective:** write, test and explain production-style data pipelines, the core job of a Data Engineer on Skywise.

**The priority order I follow** (advice from a Skywise expert who works with Airbus teams):

```
1. SQL            the common language of analysts, engineers and business teams
2. Data architecture   cardinality, layers, quality rules, the medallion model
3. Python & big data   pandas → Polars → PySpark, batch vs incremental, table formats
4. Orchestration       Airflow DAGs, retries, alerts
```

## Skills matrix

| Skill | Target level by Sep 2027 | How I build it | Status |
|---|---|---|---|
| SQL: joins, GROUP BY / HAVING, window functions, CTEs, NULL handling | Fluent, without help | Daily practice on aviation-style tables; Aerolisa transforms in SQL | 🔄 |
| Data modelling: cardinality (1-1, 1-N, N-N), star schema, SCD type 2 | Working proficiency | Aerolisa object model; [modelling notes](../knowledge/data-engineering.md#data-modelling) | 🔄 |
| Python for data: pandas, clean functions, logging, pytest | Fluent | Aerolisa code, tested | 🔄 |
| Polars | Working proficiency | Silver layer of the medallion pipeline | ⬜ |
| PySpark: lazy evaluation, joins, shuffles, partitioning, skew | Working proficiency | Aerolisa Foundry transforms | ⬜ |
| Medallion architecture (bronze / silver / gold) | Working proficiency | Medallion pipeline on C-MAPSS | ⬜ |
| Batch vs incremental, watermarks, idempotency | Working proficiency | Incremental loads in the medallion pipeline; Foundry incremental transforms | ⬜ |
| Table and file formats: Parquet, Delta Lake, Apache Iceberg | Working proficiency | Gold tables in the medallion pipeline | ⬜ |
| Orchestration: Apache Airflow | Working proficiency | DAG for the medallion pipeline | ⬜ |
| Data quality checks and monitoring | Working proficiency | Checks at every layer; monitoring dashboard | ⬜ |
| Git workflow: branches, pull requests, reviews | Fluent | All projects on GitHub | ✅ |
| Docker | Basic | Containerised medallion pipeline | ⬜ |
| Visualisation: Power BI | Working proficiency | ALTEN internship | ✅ |

## Project: medallion pipeline, from A to Z, outside Foundry

The expert's advice was explicit: **prove the concepts, not only a tool.** Build a complete pipeline without Palantir Foundry; the same ideas then apply inside it.

| Layer | Content (C-MAPSS engine data) | Checks |
|---|---|---|
| **Bronze** | Raw sensor readings, as they arrive, in a mini data lake (MinIO, S3-compatible) | Schema, row count vs usual volume, freshness |
| **Silver** | Cleaned time series: typed, deduplicated, constant sensors removed, one row per engine × cycle | No NULL keys, unique keys, values within physical ranges |
| **Gold** | Engine health indicators and RUL model features (30-cycle rolling windows) | Minimum history per engine, sanity checks on KPIs |

**Steps:** storage (MinIO) → transformation (dbt, Polars or Spark) → orchestration (Airflow) → decoupling (nothing depends on one platform).

**Bonus to match real expectations:** data tests, incremental processing, Parquet with Delta Lake or Iceberg, a small monitoring dashboard.

This reproduces the Skywise loop at small scale: sensor data → algorithm → recommendation.

## Milestones

| Milestone | Target | Status |
|---|---|---|
| Advanced SQL without help (window functions, CTE deduplication, joins and aggregates) | Jan 2027 | 🔄 |
| PySpark in Aerolisa's Foundry transforms | Jan 2027 | ⬜ |
| Medallion pipeline: bronze and silver layers | Feb 2027 | ⬜ |
| Medallion pipeline: gold layer, Airflow DAG, data tests | Mar 2027 | ⬜ |
| Incremental loads, Delta Lake or Iceberg, monitoring dashboard | Apr 2027 | ⬜ |

## Evidence

- [Aerolisa](https://github.com/xdmanflow/Aerolisa): Foundry transforms and the medallion pipeline (as they are built)
- [Turbofan-RUL](https://github.com/xdmanflow/Turbofan-RUL): Python and ML research code
- Notes: [knowledge/data-engineering.md](../knowledge/data-engineering.md) · [knowledge/data-quality-governance.md](../knowledge/data-quality-governance.md)

<sub>Back to [README](../README.md)</sub>
