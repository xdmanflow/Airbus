# Engineering practice: how data teams work

> What I learned about the non-technical side of the job, mostly from a Skywise expert who works with Airbus teams. His summary: the job is as much business as technical.

## Where juniors get stuck

Juniors rarely get stuck on technique. They get stuck on the business, rigour and communication.

| Trap | What a team expects | What I do |
|---|---|---|
| Not understanding the business | Define the need before coding | Restate the business problem at the start of every project |
| Coding without adding value | Every deliverable serves a decision | Link each pipeline to a KPI or a user |
| Lacking rigour | Everyone writes code the same way and speaks the same language, so there are tests | Follow team conventions; test every transformation |
| Not daring to speak | A junior raises a hand on sensitive topics (GDPR data, processes) | Flag a doubt about a dataset, a rule or a process early |

## What separates two similar profiles

| Criterion | What it means | How I show it |
|---|---|---|
| Use of AI | Use AI without losing the fundamentals; beware of dependency | I explain what I wrote myself and how I check what an assistant proposes |
| Fundamentals | SQL, layers, quality, data architecture, medallion model | The medallion pipeline and its architecture choices |
| Curiosity | Wanting to learn, technology watch | Polars, Iceberg, incremental processing, the AI FDE model, and what I tested of each |
| What experience taught me | Lessons, not just tasks | ALTEN: cleaning, analysis and visualisation for France and UK stakeholders |
| Code philosophy | Reading code, not only writing it | Clean, tested, documented code; reviewing someone else's code |
| Professional attitude | Behaviour, communication, reliability | Clarity, punctuality, following up on advice received |
| Technical + business | Both, not one | Linking each technical choice to an aviation stake: AOG, availability, penalties |

## How teams collaborate

- **Agile:** gather the need, short iterations, regular feedback from the business. A sprint is a fixed period (often two weeks) ending with a review and a retrospective; the daily stand-up is one minute: yesterday, today, blockers.
- **SAFe (agile at scale):** several companies (Airbus, subcontractors) work together with clear relationships between deliverable producers.
- **Analyst ↔ engineer contract:** the analyst specifies what is needed, the engineer guarantees reliable data, official tests validate it.
- **Monitoring dashboards** on pipelines and data, to catch problems before the business does.
- **Complex production** is where analyst and engineer teams split; on small projects, one person may do both.

## Working across countries

Over-communicate in writing: clear messages, shared documents, decisions recorded so nobody depends on a meeting they missed. Agree on response times, respect time zones, switch to a short call as soon as something is ambiguous.

## Apprentice habits: school weeks and company weeks

Before each school period: a written handover (status of every task, what is blocked, who to contact), nothing half-deployed. During school: check messages at agreed times. Back at the company: a short catch-up with the manager.

## When I don't know

"Good question. I don't want to guess. I'll check and come back to you by [time]." Then actually do it. With data, a confident wrong answer is much worse than "I'll check".
