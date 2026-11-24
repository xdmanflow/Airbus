# Palantir Foundry & AIP

> My notes from Palantir's learning paths, certifications and hands-on work.

## Building blocks

| Block | My explanation |
|---|---|
| **Data Connection** | Brings sources (SAP, databases, files) into Foundry through agents and syncs |
| **Datasets** | Store the data, versioned by transaction; every change is traceable |
| **Branches** | Like Git: change a pipeline on a branch, build it, compare outputs, then merge, without touching production |
| **Lineage** | The graph from source to app: before changing a dataset, I see everything downstream |
| **Pipeline Builder** | Visual pipelines, fast to build and read |
| **Code Repositories** | Python / PySpark / SQL transforms, with Git branches, pull requests and tests |
| **Ontology** | The semantic layer: datasets become business objects (Aircraft, Engine, Flight), with links and actions |
| **Workshop / Slate** | Operational apps on Ontology objects (Workshop fast, Slate custom) |
| **Contour / Quiver** | Point-and-click analysis; Quiver for time series and object analysis |
| **Health checks** | Rules on datasets (freshness, schema, row count, build status) that alert the owner |
| **AIP / AIP Logic** | LLMs wired into the Ontology. Teams can call LLMs directly or go through AIP |

**Pipeline Builder or Code Repositories?** Pipeline Builder for visual, quick transformations; Code Repositories for complex logic, tests and code review.

**ELT, not ETL:** Foundry loads raw data first and transforms it on the platform. Raw data stays untouched, so logic can be replayed and lineage stays clear.

## A Python transform

A transform declares its inputs and output; Foundry handles the build and the lineage.

```python
from transforms.api import transform_df, Input, Output
from pyspark.sql import functions as F


@transform_df(
    Output("/Aerolisa/clean/engine_snapshots"),
    raw=Input("/Aerolisa/raw/engine_snapshots"),
)
def compute(raw):
    return (
        raw
        .filter(F.col("engine_serial").isNotNull())
        .withColumn("flight_date", F.to_date("timestamp_utc"))
        .dropDuplicates(["engine_serial", "timestamp_utc"])
    )
```

**Incremental or snapshot?** A snapshot rebuild recomputes everything. An incremental transform (Palantir's incremental decorator) processes only new transactions: much cheaper for inputs that only grow, like daily sensor snapshots. Its limits matter: late or modified rows need explicit handling, and a periodic full rebuild still catches corrections.

## Ontology: modelling an aircraft fleet

| Object type | Key properties | Links |
|---|---|---|
| Aircraft | MSN, registration, type, operator | Flights, Installations |
| Engine | engine serial number (ESN), type, predicted RUL | Installations, Sensor readings, Alerts |
| **Installation** | ESN, MSN, position, installed on, removed on | Engine ↔ Aircraft, over time |
| Flight | flight number, date, cycles | Aircraft, Fault messages |
| Fault message | code, ATA chapter, timestamp | Flight, Maintenance event |
| Maintenance event | task, date, outcome | Closes Fault messages; Aircraft, Engine |
| Alert | severity, cause, status | Engine |

**The tricky part: engines move between aircraft.** The engine serial number is the key, not the aircraft. An **Installation** object with install and removal dates links an Engine to an Aircraft over time, so every data point belongs to the right engine.

**Actions:** acknowledge an alert, create a work order. The engineer's response is written back, so we learn which alerts were useful.

**Why the Ontology matters more than tables for operations users:** a maintenance engineer does not think in tables; they think "this aircraft, its open faults, what do I do next". The Ontology lets apps speak that language, with permissions on each object.

## Multi-airline platforms: permissions

- Each airline's data lives in its own projects, with restricted access.
- **Markings** travel with the data through the pipeline, so derived datasets stay protected.
- Least privilege, everything audited; cross-airline benchmarks only use data the agreements allow, aggregated or anonymised.

## AIP and LLMs in an aviation context

- **Where they help:** searching maintenance logs and technical documents in plain language, summarising a fault's history, drafting a work order for an engineer to review.
- **Risks:** invented answers, data leaking between airlines, over-trust.
- **Guardrails:** ground answers in Ontology data with links to sources, keep a human in the loop, never let a model take airworthiness decisions.

## The Forward Deployed Engineer mindset

Palantir's philosophy since around 2010: an engineer embedded with the customer, solving their problems with data. The **AI FDE** applies this to generative AI. On the consulting side, understanding needs is not enough: you also create them, by proposing proofs of concept. It takes a double IT / business profile, plus knowledge of regulation.

## Hands-on log

| Date | What I built | Lesson |
|---|---|---|
| 2026 | Speedrun: end-to-end workflow, raw data to app | Small first build, then read the docs around the problems hit |
| Nov 2026 | Data Connection deep dive | How sources and syncs bring data into Foundry |
| | Aerolisa C-MAPSS pipeline | *to come* |

## Sources

- Palantir learning paths and certifications (learn.palantir.com)
- Palantir Foundry documentation (palantir.com/docs)
- [Business Wire: Palantir and Airbus extend strategic collaboration (10 Feb 2026)](https://www.businesswire.com/news/home/20260210301221/en/Palantir-and-Airbus-Extend-Strategic-Collaboration/)
