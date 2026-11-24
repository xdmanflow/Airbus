# Aerospace domain

> A Skywise engineer does not need to be a mechanic, but needs the vocabulary. A data engineer must understand how the aircraft is designed, built, operated and maintained; otherwise nobody knows what a data point means.

## The aircraft lifecycle behind the data

| Phase | Data it produces | Example use |
|---|---|---|
| Design & engineering | Configurations, specifications, test data | Physical limits for quality checks |
| Production & supply chain | Plans, parts, supplier deliveries | Plan vs actual, bottlenecks (Skywise's first use case, on the A350) |
| Operation | Sensor parameters, fault messages, flight plans | Health monitoring, fuel efficiency |
| Maintenance | Logbook entries, work orders, removals | Predictive maintenance, reliability |

**Configuration matters:** two aircraft of the same type can differ by modifications and parts. Recommendations must link the exact aircraft to the changes applied to it.

## Maintenance

**MRO:** Maintenance, Repair and Overhaul.

- **Line maintenance:** between flights, at the gate or overnight: checks, small fixes, part swaps.
- **Base maintenance:** heavy work in a hangar (e.g. a C-check); the aircraft is out of service for days or weeks.

| Approach | Rule |
|---|---|
| Corrective | Fix it after it fails |
| Preventive | Replace at a fixed interval (flight hours, cycles, calendar), needed or not |
| **Predictive** | Replace based on actual condition and predicted degradation from data: avoids both surprise failures and replacing healthy parts |

**AOG (Aircraft On Ground):** the aircraft cannot fly because of a technical problem. Costs pile up fast: cancellations, passenger compensation, crews out of position, parts flown in urgently, an expensive asset earning nothing. Avoiding a few AOG events a year justifies a lot of data work.

## How aircraft data reaches the ground

Thousands of sensor parameters, fault and warning messages, in-flight reports, ADS-B position broadcasts, and on the ground, technical logbook entries and maintenance records. In flight via datalink (ACARS); after landing via wireless download at the gate or manual transfer.

## ATA chapters

A standard numbering of aircraft systems, used across manuals and airlines (e.g. 21 air conditioning, 32 landing gear, 72 engine). In data, the ATA chapter is a **shared key** to group faults, tasks and reliability statistics by system, across fleets.

## What airlines measure

- **Dispatch reliability:** share of departures without a technical delay
- On-time performance
- Aircraft availability and utilisation
- Fuel burn per seat
- Maintenance cost per flight hour
- Number of AOG events

A good Skywise pipeline moves at least one of these.

## Who cares about a Skywise project

| Stakeholder | Wants |
|---|---|
| Airline maintenance engineers | Fewer unscheduled removals, earlier warnings |
| Airline operations | Available aircraft |
| Airbus customer support | Faster troubleshooting |
| Airbus engineering | Fleet-wide insight to improve designs |
| Suppliers | Component reliability data |
| Skywise product team | Adoption: if nobody uses the tool, the pipeline is worthless |

## Data and decarbonisation

Most emissions come from burning fuel, so data helps on fuel first: more efficient trajectories and flight planning (now inside Skywise through Navblue), better taxi and cost-index decisions. Maintenance helps too: an engine wash at the right time, a fix that reduces drag. And data tracks emissions and sustainable fuel use for reporting.

## Predictive maintenance on C-MAPSS: what I know

| Question | Answer |
|---|---|
| What is C-MAPSS? | NASA simulation data for a generic turbofan engine run to failure, not data from a specific aircraft. FD001 has one operating condition and one fault mode |
| Why cap the RUL (~125 cycles)? | Early in life, degradation is not visible; a piecewise-linear target avoids learning noise |
| Why the NASA score, not only RMSE? | It penalises late predictions more than early ones: a failure predicted too late is more dangerous |
| How to avoid leakage? | Split train / validation by engine, never by row; rolling features on the past only |
| What would break first in production? | The data: many operating conditions, missing sensors, engines moving between aircraft, rare real failures, maintenance resetting the clock, drift. Start with data quality and a simple, explainable baseline |

## Glossary

| Term | Meaning |
|---|---|
| ACARS | Aircraft Communications Addressing and Reporting System: datalink between aircraft and ground |
| ACMS | Aircraft Condition Monitoring System: generates in-flight condition reports |
| ADS-B | Automatic Dependent Surveillance–Broadcast: position broadcasts (OpenSky data) |
| AOG | Aircraft On Ground: grounded until repaired |
| ATA chapter | Standard numbering of aircraft systems |
| C-check | Heavy base maintenance check |
| Dispatch reliability | Share of departures without a technical delay |
| EGT margin | Exhaust Gas Temperature margin: how far an engine runs below its temperature limit; shrinks as it degrades |
| ESN | Engine Serial Number |
| Flight cycle | One take-off and one landing |
| MCC | Maintenance Control Centre |
| MRO | Maintenance, Repair & Overhaul |
| MSN | Manufacturer Serial Number (of an aircraft) |
| OI / OR | Operational Interruption / Operational Reliability |
| RUL | Remaining Useful Life |
| Shop visit | Engine removed and sent to a workshop; restores performance margins |
| Turnaround | Time to prepare an aircraft between two flights |

## Sources

- NASA Prognostics Center of Excellence: C-MAPSS turbofan engine degradation dataset
- Saxena et al. (2008), *Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation*
- Airbus Services and Skywise public pages
- Advice session with a Skywise expert working with Airbus teams (November 2026)
