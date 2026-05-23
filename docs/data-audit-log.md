DAIKIBO TELEMETRY DATA — AUDIT LOG
Analyst: Oluwadunmininu Deborah Oluremi
Date: 23 - May - 2026
Client: Macora Industries (via Deloitte/Macora engagement)

DATASET OVERVIEW
- File: daikibo-telemetry-data.json
- Total Records: 160,704
- Date Range: 2021-04-30 to 2021-05-09

DATA QUALITY OBSERVATIONS
1. [ANOMALY] Client stated data covers "one month (May 2021)" 
   but timestamps span only ~10 days (April 30 – May 9).
   Impact: Downtime figures may underrepresent actual monthly exposure.
   
2. [STRUCTURE] JSON is nested (2 levels deep). 
   Fields: location.factory, location.city, data.status, data.temperature.
   Action Required: All schema levels must be expanded on import into Tableau.

3. [COMPLETENESS] No null or missing values detected across all 160,704 records.
   Status field contains exactly 2 values: "healthy" / "unhealthy". Clean binary.

4. [DISTRIBUTION] Records are perfectly balanced across 4 factories 
   (40,176 each) — consistent with 9 machines × 10-min intervals × ~31 days.

5. [OUTLIER] Unhealthy records = 103 of 160,704 (0.064%). 
   LaserWelder and LaserCutter account for 88% of all failures.