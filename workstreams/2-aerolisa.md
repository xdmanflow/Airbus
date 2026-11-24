# Workstream 2 · Aerolisa (flagship)

**Aerolisa: The Data & AI System for Aerospace Operations** is a Skywise-style project built on public aerospace data. Its job is to show that I can do Skywise work: **pipelines feeding an object model that feeds an app a maintenance team could use.**

**Rule:** one finished slice beats five started modules. Aerolisa is designed to host several use cases; slice 1 is finished end to end before anything else starts.

---

## Slice 1 · Predictive maintenance on Foundry

**The problem:** predict how many cycles an engine has left, so maintenance is planned rather than urgent.

This is Skywise Fleet Performance+'s predictive maintenance idea, in miniature, on public data. It started as my CESI research project on turbofan remaining useful life (RUL) and is now Aerolisa's first module.

### Architecture

```mermaid
flowchart LR
    A[Raw C-MAPSS files] --> B[PySpark transforms<br/>+ data health checks]
    B --> C[Features<br/>30-cycle rolling windows]
    C --> D[RUL model<br/>+ P10–P90 interval]
    D --> E[Ontology<br/>Aircraft · Engine · Flight · Maintenance alert]
    E --> F[Workshop app<br/>risk-ranked inspection queue]
```

| Aerolisa layer | Skywise counterpart |
|---|---|
| Ingestion + PySpark transforms with health checks | Code Repositories / Pipeline Builder, data health checks |
| Features and RUL model | Predictive maintenance algorithms |
| Ontology: Aircraft, Engine, Flight, Maintenance alert | Skywise's aircraft and maintenance object model |
| RUL converted into days using each aircraft's utilisation | Turning a prediction into a maintenance plan |
| Inspection queue sorted by risk, in Workshop | Operational apps used by maintenance engineers |

### Method

| Choice | Why |
|---|---|
| **C-MAPSS FD001** first | One operating condition and one fault mode: the right place to compare methods fairly |
| **RUL capped (piecewise linear, ~125 cycles)** | Early in life an engine shows no visible degradation; capping avoids learning noise |
| **Split by engine, never by row** | Otherwise the model memorises engines it has already seen (data leakage) |
| **Rolling features computed on the past only** | Same reason: no information from the future |
| **RMSE and the NASA score** | The NASA score penalises late predictions more than early ones: predicting a failure too late is more dangerous |
| **Prediction intervals (P10–P90)** | A maintenance team needs the uncertainty, not just a number |

### Results so far (FD001)

| Model | RMSE | NASA score |
|---|---|---|
| **Gradient boosting** (30-cycle rolling features) | **14.0** | **292** |
| LSTM | 15.4 | 423 |

P10–P90 intervals cover **81%** of true values against an 80% target, so the uncertainty is well calibrated.

**Lesson:** with limited data and good features, the simpler model generalises better. Data and feature quality matter more than model complexity.

**Said up front:** C-MAPSS is NASA simulation data for a generic turbofan, not data from a specific aircraft type.

### What I will show in 5 minutes

1. **The problem:** cycles left per engine, so maintenance is planned rather than urgent.
2. **The pipeline:** PySpark transforms from raw files to features, with data health checks.
3. **The object model:** Aircraft, Engine, Flight, Maintenance alert, and how they link.
4. **The app:** RUL per engine and an alert list sorted by urgency.
5. **The result:** model error and what the alert threshold means for a maintenance team.

### Milestones

| Milestone | Target | Status |
|---|---|---|
| Research: classical ML vs LSTM on FD001, prediction intervals | Nov 2026 | ✅ |
| C-MAPSS ingested into Foundry, PySpark transforms with health checks | Dec 2026 | ⬜ |
| Ontology: Aircraft, Engine, Flight, Maintenance alert | Jan 2027 | ⬜ |
| Workshop inspection queue and one alert, end to end | Jan 2027 | ⬜ |
| Demo video and English documentation | Jan 2027 | ⬜ |
| CESI research report and poster | 12 & 20 Apr 2027 | ⬜ |

---

## Slice 1b · Medallion pipeline outside Foundry

The same engine data through a platform-independent **bronze → silver → gold** pipeline: MinIO storage, dbt / Polars / Spark transformations, Airflow orchestration, data tests, incremental loads.

Why: to prove I master the concepts, not just one tool. Details in [Workstream 1](1-data-engineering.md#project-medallion-pipeline-from-a-to-z-outside-foundry).

---

## Later slices (only after slice 1 is finished)

| Slice | Public data | Skywise parallel |
|---|---|---|
| Flight operations | OpenSky Network (ADS-B) | Navblue flight operations, now part of Skywise |
| European traffic | Eurocontrol | Network and punctuality analytics |
| Orders & deliveries | Airbus published orders and deliveries | Production plan vs actual, the question Skywise was first built for on the A350 |

## Rules

- **Public data only:** C-MAPSS, OpenSky, Eurocontrol, Airbus's published figures. Never ALTEN or client data.
- **"Skywise-style", never Skywise:** no Airbus logos, no implied endorsement.
- **Status is updated when work is done**, never before.

→ [github.com/xdmanflow/Aerolisa](https://github.com/xdmanflow/Aerolisa) · Research code: [github.com/xdmanflow/Turbofan-RUL](https://github.com/xdmanflow/Turbofan-RUL)

<sub>Back to [README](../README.md)</sub>
