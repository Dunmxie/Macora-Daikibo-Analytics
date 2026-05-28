# Data Reconnaissance

## Objective
Before any visualisation, understand the raw data structure.
This prevents misconfigured imports, wrong field references,
and shallow analysis.

## Dataset Overview

| Property | Detail |
|---|---|
| Source | Daikibo Industries — IoT Telemetry System |
| Format | JSON (single file) |
| Size | ~58MB |
| Period | May 2021 (one full month) |
| Factories | 4 global locations |
| Machine types | 9 per factory |
| Message frequency | Every 10 minutes per machine |
| Estimated records | ~260,000+ |

## Field Map

| Field | JSON Path | Type | Business Role |
|---|---|---|---|
| Machine ID | `deviceID` | String | Unique machine identifier |
| Machine type | `deviceType` | String | One of 9 machine categories |
| Timestamp | `timestamp` | Unix ms | 10-min interval signal |
| Country | `location.country` | String (nested) | Geographic context |
| City | `location.city` | String (nested) | Geographic context |
| Factory | `location.factory` | String (nested) | Primary grouping — Q1 answer |
| Section | `location.section` | String (nested) | Sub-factory granularity |
| Status | `data.status` | String (nested) | `healthy` or `unhealthy` |
| Temperature | `data.temperature` | Integer (nested) | Thermal signal — bonus insight |

## Structural Flags

- `location` and `data` are **nested JSON objects**
- Analytics tool *(in this case, Tableau)* must expand all schema levels on import
- Failure to do so silently drops the most critical fields
- `data.status` is the core binary signal driving all downtime calculations

## Business Questions This Data Must Answer

1. Which factory had the most machine downtime in May 2021?
2. Which machine types failed most often in that factory?

## Analyst Note

The presence of `data.temperature` is outside the task scope
but presents an opportunity. Thermal spikes correlating with
`unhealthy` status events could indicate root causes beyond
simple failure counts a finding that moves the analysis
from *descriptive* to *diagnostic*.

This will be explored in a supplementary insight layer.