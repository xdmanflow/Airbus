# Airbus Skywise 2027 · Data Engineering Flight Plan

I'm **Manil Doudou**, a fourth-year computer science engineering student at **CESI Toulouse** (AI & Data Science track, taught in English, top of class 2025–2026) and a **Data Analyst intern at ALTEN Toulouse**, in the Aeronautics, Space & Defence department.

This repository is my public, structured plan to join **Airbus as a Data Engineer apprentice in September 2027**, on the **Skywise** platform and its **Palantir Foundry** stack. It shows what I know, what I am building, and what I still have to prove.

---

## At a glance

| | |
|---|---|
| **Target role** | Data Engineer apprentice, Airbus Skywise or an Airbus data team working on Skywise |
| **Contract** | Contrat de professionnalisation, 12 months, final year of the CESI engineering degree (RNCP40612) |
| **Dates** | September 2027 → August 2028 · Toulouse |
| **Flagship proof** | [Aerolisa](workstreams/2-aerolisa.md): a Skywise-style data & AI system on Foundry, plus a platform-independent medallion pipeline |
| **Long-term direction** | Data & AI engineering for aerospace operations, growing towards data architecture and AI |

## Why Skywise, and why now

- **Skywise is where aircraft data changes operations.** Launched in 2017 with Palantir, it connects **12,000+ aircraft** and helps airlines anticipate failures instead of discovering them at the gate.
- **2026 made it a company.** On 1 April 2026 Airbus merged Skywise digital solutions and **Navblue** into one Skywise subsidiary (~750 people); in February 2026 Palantir and Airbus signed a **multi-year extension** of their Skywise collaboration; in November 2026 Airbus digital flight training moved into the Skywise portfolio.
- **The core use case is mine already.** Skywise Fleet Performance+ covers health monitoring, predictive maintenance and reliability. Aerolisa's first module predicts the remaining useful life of turbofan engines: the same problem, at student scale, on public data.

Details and sources: [knowledge/airbus-skywise.md](knowledge/airbus-skywise.md)

## What I learned, in one sentence each

Most of this comes from Palantir's learning paths, real Airbus job postings, and advice from a Skywise expert who works with Airbus teams.

1. **Business need first.** A pipeline that does not serve a decision (an alert, a KPI, an inspection) has no value. Juniors rarely fail on technique; they fail on understanding the business.
2. **SQL before Python.** Most transformations are written in SQL, the common language of analysts, engineers and business teams. Python and PySpark come next.
3. **Architecture beats tools.** Cardinality before every join, data in layers (bronze → silver → gold), quality rules at each layer. The same concepts work inside and outside Foundry.
4. **Incremental is the default at scale.** Aircraft data never stops arriving: batch vs incremental, late data, idempotent reruns, Parquet, Delta Lake and Iceberg.
5. **Rigour is a team skill.** Same conventions, tests on every transformation, code others can read, and raising a hand early on GDPR, export control or a doubtful number.
6. **Predictive maintenance is simple models on excellent data.** The value is in data quality and the link to maintenance decisions, not model complexity.
7. **Use AI without losing the fundamentals.** I explain what I wrote myself and how I check what an assistant proposes.

The full notes are in **[knowledge/](knowledge/)**.

## Strategy: five workstreams

| # | Workstream | Focus | Outcome by Sep 2027 |
|---|---|---|---|
| 1 | [Data Engineering Foundations](workstreams/1-data-engineering.md) | SQL first, data architecture, PySpark, incremental processing, orchestration | Write and explain production-style pipelines without help |
| 2 | [Aerolisa (flagship)](workstreams/2-aerolisa.md) | Predictive maintenance slice on Foundry · medallion pipeline outside Foundry | One finished, demonstrable slice, end to end |
| 3 | [Palantir Foundry & AIP](workstreams/3-palantir-foundry.md) | Certifications, Ontology, transforms, Workshop | Foundry Aware certified; own pipeline and Ontology in a demo |
| 4 | [Experience, English & Communication](workstreams/4-experience-english.md) | ALTEN internship, TOEIC, explaining data to non-specialists | Internship defended · TOEIC C1 · clear 2-minute stories |
| 5 | [Airbus & Skywise Readiness](workstreams/5-airbus-skywise.md) | Domain knowledge, network, applications | Fluent in Skywise, MRO and the 2027 application process |

## Timeline

| Phase | Period | Focus |
|---|---|---|
| **1 · Secure & certify** | Nov 2026 – Jan 2027 | Foundry Aware · Aerolisa Foundry slice · internship report and recommendations |
| **2 · Prove** | Feb – Apr 2027 | TOEIC C1 · internship defence · medallion pipeline · research report and poster |
| **3 · Apply** | Feb – Jun 2027 | "ALT 2027" Skywise offers · video interviews · technical tests |
| **4 · Transition** | Jul – Sep 2027 | Contract signed · onboarding preparation · **start at Airbus** |

Milestones, Gantt chart and my definition of "ready": **[ROADMAP.md](ROADMAP.md)** · Purpose, career compass and target stack: **[VISION.md](VISION.md)**

## Flagship project: Aerolisa

**Aerolisa: The Data & AI System for Aerospace Operations** is a Skywise-style project built on public data. It is designed to host several aerospace use cases; I am finishing **one slice end to end** before starting the next.

**Slice 1, predictive maintenance** (NASA C-MAPSS turbofan simulation data):

```
raw C-MAPSS files ─► PySpark transforms + data health checks ─► features (30-cycle windows)
        ─► RUL model with P10–P90 interval ─► Ontology: Aircraft · Engine · Flight · Maintenance alert
        ─► Workshop app: risk-ranked inspection queue
```

**Slice 1b, platform-independent:** the same engine data through a **medallion pipeline** (MinIO → dbt / Polars / Spark → Airflow), to prove the concepts without Foundry.

→ [Workstream page](workstreams/2-aerolisa.md) · [Aerolisa repository](https://github.com/xdmanflow/Aerolisa) · [Original research code: Turbofan-RUL](https://github.com/xdmanflow/Turbofan-RUL)

## Credentials

| Credential | Issuer | Status |
|---|---|---|
| Introduction to Foundry & AIP for Enterprise Organizations | Palantir | ✅ Earned |
| Foundry & AIP Builder Foundations | Palantir | ✅ Earned |
| Speedrun: Your First End-to-End Workflow on Foundry | Palantir | ✅ Earned |
| Deep Dive: Creating Your First Data Connection | Palantir | ✅ Earned (Nov 2026) |
| Foundry Aware | Palantir | 🔄 Target: Dec 2026 |
| TOEIC Listening & Reading, C1 level | ETS | 🔄 Target: Feb 2027 |
| Top of class, CESI Toulouse (2025–2026) | CESI | ✅ July 2026 |

## Repository map

```
├── README.md          You are here
├── VISION.md          Purpose, career compass, target tech stack, working principles
├── ROADMAP.md         Phases, milestones, Gantt chart, skills gap, definition of ready
├── workstreams/       One page per workstream: objective, milestones, evidence
├── knowledge/         My notes: Skywise, Foundry, data engineering, quality & governance, aerospace
├── case-studies/      Design exercises on Skywise-style problems
└── progress/          Monthly progress updates
```

## Contact

**Manil Doudou** · [LinkedIn](https://www.linkedin.com/in/manil-doudou) · [GitHub](https://github.com/xdmanflow)
