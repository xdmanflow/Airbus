# Data quality, governance & security

> In aviation, a wrong number can ground an aircraft. These are the rules I apply to data I do not own.

## Marketing error vs maintenance error

A marketing error wastes a campaign. A maintenance error can lead to a wrong decision about an aircraft: a safety and regulatory issue. So the bar is different:

- every figure traceable to its source;
- every change reviewed and validated;
- pipelines that **fail loudly** instead of publishing doubtful data;
- data supports engineers' decisions; it never replaces the certified maintenance process.

## Sensor data problems

I never fix data silently: raw stays untouched, quality flags are added.

| Problem | Handling |
|---|---|
| **Gaps** | Flag them; fill only short gaps, only for analytics, never for safety decisions |
| **Frozen values** | Same value for N consecutive readings → mark the sensor as suspect |
| **Out of range** | Compare with physical limits from engineering specs; quarantine those readings |
| **All of them** | Report the rate per aircraft: a rising rate is itself a signal |

## Checks, layer by layer

| Where | Checks |
|---|---|
| Ingestion | Schema, freshness, row count vs usual volume |
| After cleaning | No NULL keys, unique keys, valid ranges, every engine serial number exists in the reference table |
| After joins | Every record matches exactly one reference row; unmatched rate tracked |
| Before output | Sanity checks on final KPIs; a sudden spike is checked before anyone is alerted |

In Foundry: health checks and data expectations. A failed critical check **stops the build**.

## Investigating "30% fewer flights yesterday"

1. **Is the drop real?** Check raw counts at the source.
2. If the raw data is fine, **follow the lineage** step by step to find where rows disappear; check recent changes to filters and joins.
3. Often, one airline's file simply **arrived late**.
4. Meanwhile, **tell users** the figure is under investigation, with a time for the next update.
5. After the fix: **backfill**, and add a volume check so it is caught automatically next time.

## Time

Convert everything to **UTC** at ingestion; keep the original value and its source time zone in separate columns; document the reference clock. Watch daylight-saving changes in ground systems, and join events with a small time tolerance because clocks drift.

## Ownership and multi-airline data

Airline data belongs to the airline:

- each airline's data separated, with its own access rules;
- the data-sharing agreement defines what may be used and for what;
- least privilege, logged access;
- fleet-wide benchmarks only on data the agreements allow, aggregated or anonymised;
- if an airline leaves, we must know exactly where its data went: that is why lineage matters.

## Personal data (GDPR)

There is more personal data in aircraft data than people think: crew identifiers linked to flight data, technician names in maintenance logs, sometimes passenger data. GDPR requires a legal basis, minimisation, limited retention and controlled access. In practice: remove or pseudonymise identifiers the use case does not need. Flight data linked to pilots is especially sensitive.

## Export control

Some technology and technical data have legal access limits (EU and US regulations, for example). At Airbus, some engineering data is restricted by project and by person. Respect the data's classification, never move data out of approved environments, and **ask when unsure**.

## Documentation

- Dataset names by layer (raw, clean, curated)
- A README per pipeline: purpose, sources, owner, schedule
- Column descriptions in the dataset itself
- Tests that read like a specification
- A short decision log: the "why" is what people lose first

Written for someone who joins six months after I leave.

## A junior's duty: speak up

A junior must raise a hand on sensitive topics (GDPR data, processes, a doubtful rule) early, rather than hope it is fine.

## Sources

- GDPR (Regulation (EU) 2016/679)
- Palantir Foundry documentation on health checks, markings and permissions
- Advice session with a Skywise expert working with Airbus teams (November 2026)
