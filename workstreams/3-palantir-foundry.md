# Workstream 3 · Palantir Foundry & AIP

**Objective:** be operational on Foundry before day one: data connection, transforms, Ontology and applications, the building blocks Skywise runs on.

**What the badges do not prove:** that I can write PySpark transforms or model an Ontology. Aerolisa has to show that.

## Certifications

| Certification | Status |
|---|---|
| Introduction to Foundry & AIP for Enterprise Organizations | ✅ Earned |
| Foundry & AIP Builder Foundations | ✅ Earned |
| Speedrun: Your First End-to-End Workflow on Foundry | ✅ Earned |
| Deep Dive: Creating Your First Data Connection | ✅ Earned (Nov 2026) |
| Foundry Aware | 🔄 Target: Dec 2026 |
| Advanced data engineering path (incremental transforms, AIP) | ⬜ Stretch: mid-2027 |

## The Skywise toolbox, by family

What a Skywise data engineer uses day to day, and what I need to be able to do with each.

| Family | Tool | What it is for | What I must be able to do |
|---|---|---|---|
| Ingestion | Data Connection | Bring sources (SAP, databases, files) into Foundry through agents and syncs | Set up a source and a sync (covered by my 4th certification) |
| Data engineering | Pipeline Builder | Visual, low-code pipelines | Chain clean, maintainable transformations |
| Data engineering | Code Repositories / Code Workbook | Python, PySpark and SQL transforms, with Git branches and reviews | Write testable transforms in the team's style |
| Semantics | Ontology | Business objects, links and actions on top of datasets | Model Aircraft, Engine, Flight and Maintenance alert |
| Analytics | Contour | Point-and-click analysis on large datasets | Filter, aggregate, build an analysis a business user can read |
| Analytics | Quiver, Foundry Time Series | Time series and object analysis | Explore sensor data over time |
| Analytics | Notebooks | Exploration and prototyping in Python | Test a hypothesis, prototype a model |
| Apps | Workshop, Slate | Operational apps (Workshop fast, Slate custom) | Turn a clean dataset into a screen people use |
| AI | AIP, AIP Logic | LLMs connected to the Ontology | Ground an LLM answer in Ontology data, with a human in the loop |

## Hands-on

| Milestone | Target | Status |
|---|---|---|
| End-to-end workflow, raw data to app (Speedrun certification) | 2026 | ✅ |
| Personal Foundry environment for Aerolisa | Dec 2026 | ⬜ |
| PySpark transforms with health checks on C-MAPSS | Dec 2026 | ⬜ |
| Ontology design and implementation (with an Installation object for engines) | Jan 2027 | ⬜ |
| Workshop inspection queue | Jan 2027 | ⬜ |
| Read Palantir's documentation on incremental transforms and their limits | Feb 2027 | ⬜ |

## How my internship work maps to Foundry

Every Foundry building block has a counterpart in what I did at ALTEN. Different objects, same job.

| Foundry block | What it does in Skywise | My ALTEN counterpart |
|---|---|---|
| Code Repositories / Pipeline Builder | Clean and join raw data into datasets | Python cleaning scripts for France and UK recruitment datasets |
| Datasets, lineage, branching | Versioned datasets, traced sources, work on branches | Learned the hard way: I now version everything daily |
| Ontology | Tables become business objects with links | Candidate, Need, Client, Recruiter, Status: the objects behind my analyses |
| Contour, Quiver | Analysis and charts on datasets | Power BI reporting for managers |
| Workshop, Slate | Operational apps for end users | Excel tools that recruiters used every day |
| Data health checks | Flag missing, late or malformed data | Listing bias and edge cases before automating CV extraction |
| AIP | LLMs wired into the Ontology | Scoping an LLM extraction pipeline, then adopting a GDPR-compliant agent |

## Evidence

- Aerolisa on Foundry: [Workstream 2](2-aerolisa.md)
- Notes: [knowledge/palantir-foundry.md](../knowledge/palantir-foundry.md)

<sub>Back to [README](../README.md)</sub>
