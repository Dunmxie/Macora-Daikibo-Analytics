# Phase 1 — Tableau Analysis & Dashboard Build

## Objective
Transform 58MB of raw IoT telemetry JSON into an interactive 
Tableau dashboard that answers two specific business questions:

1. Which factory had the most machine downtime in May 2021?
2. Which machine types failed most in that factory?

## Tool
**Tableau Desktop** — chosen for its ability to handle nested 
JSON schemas, create calculated fields from raw status signals, 
and build interactive cross-filtering dashboards without writing 
a single line of code.

---

## Step 1 — Data Import & Schema Validation

Tableau's JSON connector requires explicit schema level selection 
on import. The dataset contains two nested objects — `location` 
and `data` — that Tableau will silently drop if not expanded 
during import.

**Schema levels confirmed on import:**
- Root level — `deviceID`, `deviceType`, `timestamp`
- `location` — `factory`, `city`, `area`, `country`, `section`
- `data` — `status`, `temperature`

Failure to expand all schema levels is the single most common 
import error with nested JSON in Tableau. Validating the preview 
table before proceeding is non-negotiable.

---

## Step 2 — Calculated Field: "Unhealthy"

**Formula:**
```DAX
IF [Status] = "unhealthy" THEN 10 ELSE 0 END
```
**Business logic:**
Each machine transmits a status message every 10 minutes. 
One `unhealthy` message represents 10 minutes of potential 
production downtime since the previous transmission.

This calculated field converts a binary text label into a 
quantified, summable business metric — **minutes of downtime.**

This is the foundation of every visualisation in this dashboard.

---

## Step 3 — Chart 1: Down Time per Factory

- **Dimension (X axis):** `Factory`
- **Measure (Y axis):** `SUM(Unhealthy)`
- **Chart type:** Horizontal bar chart
- **Purpose:** Rank all 4 factories by total downtime minutes

### Result:

| Factory | Total Downtime (mins) |
|---|---|
| Daikibo Factory Seiko (Osaka, Japan) | 480 |
| Daikibo Shenzhen (Shenzhen, China) | 420 |
| Daikibo Factory Meiyo (Tokyo, Japan) | 110 |
| Daikibo Berlin (Berlin, Germany) | 20 |

**Answer to Client Question 1: Daikibo Factory Seiko recorded 
the highest downtime in May 2021 at 480 minutes — equivalent 
to one full 8-hour production shift lost.**

---

## Step 4 — Chart 2: Down Time per Device Type

- **Dimension (X axis):** `Device Type`
- **Measure (Y axis):** `SUM(Unhealthy)`
- **Chart type:** Bar chart
- **Purpose:** Rank all 9 machine types by total downtime minutes

### Global Device Type Downtime:

| Device Type | Global Downtime (mins) | Risk Level |
|---|---|---|
| LaserWelder | 480 | 🔴 Critical |
| LaserCutter | 430 | 🔴 Critical |
| HeavyDutyDrill | 70 | 🟡 Moderate |
| Furnace | 20 | 🟡 Low |
| SpotWelder | 10 | 🟢 Minimal |
| ConveyorBelt | 10 | 🟢 Minimal |
| CNC | 10 | 🟢 Minimal |
| MetalPress | 0 | ✅ Zero failures |
| AirWrench | 0 | ✅ Zero failures |

---

## Step 5 — Interactive Dashboard

Both charts combined into a single dashboard with 
**Chart 1 set as a filter.**

Selecting any factory bar in Chart 1 dynamically updates 
Chart 2 to show only that factory's device type breakdown.

This enables drill-down analysis without building separate 
views for each factory.

---

## Full Findings: Factory-Level Breakdown

| Factory | Device Type | Downtime (mins) | Factory Total |
|---|---|---|---|
| Seiko (Osaka) | LaserWelder | 480 | **480** |
| Shenzhen | LaserCutter | 390 | **420** |
| Shenzhen | ConveyorBelt | 10 | |
| Shenzhen | CNC | 10 | |
| Shenzhen | SpotWelder | 10 | |
| Meiyo (Tokyo) | HeavyDutyDrill | 70 | **110** |
| Meiyo (Tokyo) | LaserCutter | 40 | |
| Berlin | Furnace | 20 | **20** |

---

## Analytical Insights

### 1. Seiko — High Impact, Targeted Problem
480 minutes of downtime. One machine type. One factory.
LaserWelder is the sole failure source in Daikibo's worst 
performing facility. This is a targeted fix — service, 
recalibrate, or replace the LaserWelder fleet in Seiko.
High business impact. Low diagnostic complexity.

### 2. Shenzhen — Complex, Systemic Risk
420 minutes total across 4 device types. LaserCutter 
dominates at 390 minutes but 3 additional machines show 
early failure signals. Multiple failure sources in one 
facility suggest a systemic issue — maintenance culture, 
environmental conditions, or power supply instability —
rather than isolated machine faults.

### 3. Laser Technology — A Cross-Factory Fleet Risk
LaserWelder fails in Seiko. LaserCutter fails in both 
Shenzhen and Meiyo. Two distinct laser-based machine types 
failing across 3 of 4 global facilities. This pattern 
exceeds coincidence and warrants a fleet-wide review — 
vendor audit, maintenance protocol assessment, or 
technology evaluation.

### 4. Berlin — Effectively Healthy
20 minutes of downtime from a single Furnace event across 
an entire month. Lowest risk facility by a significant margin. 
Berlin's operational practices may offer a benchmark worth 
studying for other facilities.

### 5. Meiyo — Early Warning Profile
Two independent failure sources — HeavyDutyDrill and 
LaserCutter — contributing 110 minutes combined. Not critical 
today. But dual-source failure patterns historically precede 
facility-wide deterioration. Meiyo warrants proactive 
monitoring before it reaches Shenzhen-level complexity.

### 6. MetalPress and AirWrench — Zero Failures
Both machines recorded zero unhealthy events across all 
4 factories for the entire month. Either the most reliable 
machines in the fleet or candidates for utilisation review.

---

## Dashboard Screenshots

### Global View — All Factories
![Global Dashboard](../assets/screenshots/dashboard-global-view.png)

### Seiko Filtered — LaserWelder Isolated
![Seiko Filter](../assets/screenshots/dashboard-seiko-filter.png)

### Shenzhen Filtered — Multi-Device Breakdown
![Shenzhen Filter](../assets/screenshots/dashboard-shenzhen-filter.png)

---

## Answer to Client Questions

| Question | Answer |
|---|---|
| Which factory had the most downtime? | **Daikibo Factory Seiko, Osaka — 480 minutes** |
| Which machines failed most in that factory? | **LaserWelder — sole source of all Seiko downtime** |