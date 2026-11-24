# Data engineering notes

> Fundamentals first: they transfer to any platform. Examples use aviation-style tables.

## SQL

**WHERE vs HAVING:** WHERE filters rows before aggregation; HAVING filters groups after, so it can use COUNT or SUM.

**INNER vs LEFT JOIN:** INNER keeps rows matching on both sides; LEFT keeps every left row, with NULLs where there is no match (e.g. aircraft with no maintenance event yet).

**NULLs:** NULL never equals NULL, so NULL keys never match in a join. `COUNT(*)` counts all rows; `COUNT(col)` skips NULLs. GROUP BY puts all NULLs in one group. I handle them explicitly (`IS NULL`, `COALESCE`) and check NULL rates on keys before joining.

**Cardinality before every join:** is the relation 1-1, 1-N or N-N? A wrong assumption duplicates rows and silently inflates results.

### Latest maintenance event per aircraft

```sql
SELECT aircraft_id, event_type, event_date,
       CURRENT_DATE - event_date AS days_since
FROM (
    SELECT m.*,
           ROW_NUMBER() OVER (PARTITION BY aircraft_id
                              ORDER BY event_date DESC) AS rn
    FROM maintenance_events m
) t
WHERE rn = 1;
```

To include aircraft with no event at all, start from the aircraft table with a LEFT JOIN.

### Deduplicate flights, keep the latest ingested version

```sql
WITH ranked AS (
    SELECT f.*,
           ROW_NUMBER() OVER (
               PARTITION BY flight_number, aircraft_id, departure_date
               ORDER BY ingested_at DESC) AS rn
    FROM flights f
)
SELECT * FROM ranked WHERE rn = 1;
```

First define the business key. To only *find* duplicates: `GROUP BY` the key `HAVING COUNT(*) > 1`, and report the count to the data owner.

**Slow query:** read the execution plan first. Then: filter and select columns early, check join keys have the same type, look for a join that multiplies rows, avoid functions on partition or index columns, partition or index on what queries filter by (usually the date), pre-aggregate what is queried daily.

## Python

**pandas → Polars → Spark:**

| Tool | When |
|---|---|
| pandas | Small data, in memory, one machine |
| Polars (or DuckDB) | Medium data, one machine, very fast; increasingly popular |
| PySpark | Data that does not fit one machine, or must scale across a cluster; the standard for large datasets in Foundry |

**Generators and lazy evaluation:** a generator yields one item at a time, so a 50 GB file can be read line by line. Spark applies the same idea at scale: it records transformations, optimises the full plan, and runs only when an action (write, count) needs the result.

**Structuring a pipeline project:** one module per step (ingest, clean, transform, output) with small, pure functions; config in files or environment variables, never hard-coded paths; logging with row counts at each step; pytest on each transformation with tiny sample data; pinned dependencies in `pyproject.toml`; tests on every pull request.

## Data modelling

**Star schema:** state the grain first. `fact_flight`: one row per flight (block time, fuel burned, delay minutes). `fact_maintenance`: one row per task (labour hours, cost). Dimensions: aircraft, engine, airport, date, ATA chapter.

**Slowly changing dimension, type 2:** when an aircraft changes operator, close the current row (`valid_to`) and insert a new one (`valid_from`). Facts join on aircraft and on the date falling inside the validity window, so a 2024 flight still belongs to the 2024 operator: essential for leased aircraft.

**Normalise or denormalise?** Normalise where data is written (integrity, no duplicates); denormalise where it is read for analytics (fewer joins). In Foundry: clean normalised datasets underneath, wider datasets backing Ontology objects and apps.

## Pipelines

**ETL vs ELT:** ELT loads raw data first and transforms on the platform. The raw stays untouched, so logic can be replayed and lineage is clear.

**Batch vs streaming:** batch for a nightly fleet reliability report; streaming for fault messages sent in flight so ground maintenance can prepare the part before landing. Choose by how fast the decision needs the data.

**Batch vs incremental:** batch recomputes the whole dataset (simple, costly as volumes grow); incremental processes only new or changed data (essential with continuous aircraft data).

**Idempotency:** running a job twice gives the same result as once. Jobs fail and rerun, so: overwrite whole partitions, merge on keys instead of blind appends, keep transforms deterministic.

**Process only new data each day:** keep a watermark (last timestamp or file processed); re-read a lookback window (e.g. three days) because aviation data arrives late; merge on keys; schedule a periodic full rebuild to catch corrections.

**Orchestration:** a DAG where each task runs when upstream tasks succeed or on a time trigger, with retries for temporary failures, alerts to the owner, and a deadline for when data must be ready. Apache Airflow outside Foundry; schedules triggered by upstream dataset updates inside.

## The medallion architecture

| Layer | Content | Rules |
|---|---|---|
| **Bronze** | Raw data, as it arrives | Never modified; schema and freshness checks |
| **Silver** | Cleaned, typed, deduplicated | Quality checks: keys, ranges, uniqueness |
| **Gold** | Business-ready: KPIs, tables for dashboards and models | Sanity checks on outputs |

## Spark

**How a job runs:** the driver builds the plan and schedules work; executors run tasks in parallel, one per partition. Transformations (filter, join) are lazy; actions (write, count) trigger execution. Jobs split into stages at each shuffle.

**The five everyday operations:** filter rows · select columns · materialise (group by, joins: where Spark really computes and where it costs) · write a clean, reusable dataset · chain to the next step. **Filter and select early** to cut the cost of joins and aggregations.

**Shuffles:** wide transformations (groupBy, join) move data across the network. Reduce them: filter early, broadcast small tables (e.g. the aircraft reference list), pre-aggregate before joining, avoid unnecessary repartitions.

**Data skew:** one key (e.g. one airline with 40% of rows) overloads one executor. Spot it in the Spark UI (one task far longer than the rest); fix with Adaptive Query Execution's skew handling, broadcasting, or salting the key.

## Storage

| Concept | What it is | Why it matters |
|---|---|---|
| **Parquet** | Columnar, compressed file format that stores the schema | Read only needed columns, skip data with filters; CSV has no types and is read whole |
| **Partitioning** | Physical split of a table, e.g. by date | Most queries filter by date; partitioning by aircraft would create too many small files |
| **Delta Lake** | Table format adding transactions on a data lake | Foundation of lakehouse architectures |
| **Apache Iceberg** | Open table format, alternative to Delta | Popular for integration across engines (Databricks, etc.) |
| **Lake / warehouse / lakehouse** | Raw files, schema on read / modelled tables, schema on write / table formats on lake storage | Foundry behaves much like a lakehouse with versioned datasets |
| **MinIO** | S3-compatible object storage, runs locally | A mini data lake for learning projects |

## Engineering practice

- **Git:** protected main branch, one branch per task, small commits, pull requests with a description and how it was tested, review, automated tests, merge. Foundry Code Repositories follow the same model.
- **Docker:** the container fixes the Python version and dependencies, so the pipeline runs the same on a laptop, in testing and in production.

## Sources

- Advice session with a Skywise expert working with Airbus teams (November 2026)
- Spark, Polars, dbt, Airflow, Delta Lake and Apache Iceberg official documentation
- Palantir Foundry documentation
