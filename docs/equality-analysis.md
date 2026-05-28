# Phase 2 — Gender Pay Equality Analysis

## Objective
Analyse Daikibo's internal gender pay equality scores across 
all 4 factories and all job roles. Classify each role into one 
of 3 categories — Fair, Unfair, or Highly Discriminative — to 
give leadership a clear, actionable picture of where pay 
inequality is most severe.

## Context
Following a significant volume of internal complaints about 
gender-based salary disparities, Daikibo commissioned a 
forensic algorithm to quantify pay equality across every 
job role in every facility. The output is a single equality 
score per role per factory — an integer between -100 and +100, 
where 0 represents perfect equality.

This analysis classifies those scores and surfaces the patterns 
that matter most to leadership.

---

## Dataset

| Property | Detail |
|---|---|
| Source | Daikibo Forensic Tech Team |
| Format | Excel (.xlsx) |
| Columns | Factory, Job Role, Equality Score, Equality Class (added) |
| Factories covered | 4 |
| Job roles covered | Up to 11 per factory |
| Score range | -100 to +100 (0 = ideal) |

---

## Classification Framework

| Class | Condition | Meaning |
|---|---|---|
| Fair | Score > -10 AND < +10 | Acceptable pay parity |
| Unfair | Score ≤ -10 AND > -20 OR ≥ +10 AND < +20 | Meaningful disparity requiring attention |
| Highly Discriminative | Score ≤ -20 OR ≥ +20 | Severe disparity requiring immediate action |

**Note on boundary logic:** The brief's examples confirm that 
the Fair boundary is exclusive — a score of exactly -10 
classifies as Unfair, not Fair. Classification formulas 
reflect this precisely.

**Excel formula applied to every row:**
```excel
=IF(AND(C2>-10,C2<10),"Fair",IF(OR(C2<=-20,C2>=20),"Highly Discriminative","Unfair"))
```

---

## Results by Factory

### Daikibo Factory Meiyo (Tokyo, Japan)

| Job Role | Equality Score | Class |
|---|---|---|
| C-Level | -25 | 🔴 Highly Discriminative |
| VP | -26 | 🔴 Highly Discriminative |
| Director | -19 | 🟡 Unfair |
| Sr. Manager | -15 | 🟡 Unfair |
| Manager | -14 | 🟡 Unfair |
| Jr. Manager | -20 | 🔴 Highly Discriminative |
| Sr. Engineer | -5 | 🟢 Fair |
| Engineer | -8 | 🟢 Fair |
| Jr. Engineer | 3 | 🟢 Fair |
| Operational Support | -22 | 🔴 Highly Discriminative |
| Machine Operator | -7 | 🟢 Fair |

### Daikibo Factory Seiko (Osaka, Japan)

| Job Role | Equality Score | Class |
|---|---|---|
| VP | -19 | 🟡 Unfair |
| Director | -10 | 🟡 Unfair |
| Sr. Manager | -21 | 🔴 Highly Discriminative |
| Manager | -21 | 🔴 Highly Discriminative |
| Jr. Manager | -24 | 🔴 Highly Discriminative |
| Sr. Engineer | -4 | 🟢 Fair |
| Engineer | -7 | 🟢 Fair |
| Jr. Engineer | 4 | 🟢 Fair |
| Operational Support | -19 | 🟡 Unfair |
| Machine Operator | -5 | 🟢 Fair |

### Daikibo Berlin (Berlin, Germany)

| Job Role | Equality Score | Class |
|---|---|---|
| Sr. Manager | -15 | 🟡 Unfair |
| Manager | -16 | 🟡 Unfair |
| Jr. Manager | -17 | 🟡 Unfair |
| Sr. Engineer | 4 | 🟢 Fair |
| Engineer | 2 | 🟢 Fair |
| Jr. Engineer | 4 | 🟢 Fair |
| Operational Support | 0 | 🟢 Fair |
| Machine Operator | -6 | 🟢 Fair |

### Daikibo Shenzhen (Shenzhen, China)

| Job Role | Equality Score | Class |
|---|---|---|
| Sr. Manager | -21 | 🔴 Highly Discriminative |
| Manager | -19 | 🟡 Unfair |
| Jr. Manager | -20 | 🔴 Highly Discriminative |
| Sr. Engineer | -5 | 🟢 Fair |
| Engineer | -4 | 🟢 Fair |
| Jr. Engineer | 3 | 🟢 Fair |
| Operational Support | -7 | 🟢 Fair |
| Machine Operator | -7 | 🟢 Fair |

---

## Summary Dashboard

| Factory | 🟢 Fair | 🟡 Unfair | 🔴 Highly Discriminative |
|---|---|---|---|
| Daikibo Factory Meiyo | 4 | 3 | 4 |
| Daikibo Factory Seiko | 5 | 3 | 3 |
| Daikibo Berlin | 5 | 3 | 0 |
| Daikibo Shenzhen | 5 | 1 | 2 |
| **Total** | **19** | **10** | **9** |

---

## Analytical Insights

### 1. The Pay Gap Is a Leadership Pipeline Problem
Across all 4 factories, Highly Discriminative and Unfair 
classifications cluster almost exclusively at senior levels — 
C-Level, VP, Director, and Manager tiers. Ground-level roles 
(Engineers, Machine Operators, Operational Support) score 
within Fair range in the majority of cases.

This is not a company-wide pay gap. It is a leadership 
compensation gap — and that distinction changes the 
recommended fix entirely.

### 2. Meiyo Is the Most Severe Offender
Daikibo Factory Meiyo carries the most Highly Discriminative 
classifications — including C-Level at -25 and VP at -26. 
Inequality at the very top of a facility's hierarchy signals 
a governance-level failure, not an HR oversight. It suggests 
that pay decisions at the most senior levels have gone 
unscrutinised for an extended period.

### 3. Berlin Is the Benchmark
Daikibo Berlin is the only facility with zero Highly 
Discriminative classifications. Its worst scores sit in the 
Unfair band at the manager level — a problem, but a contained 
one. Berlin's compensation practices at the ground level are 
effectively equitable. Leadership should study what Berlin 
does differently and treat it as the internal standard.

### 4. Seiko Carries a Double Problem
Seiko already led all factories in operational downtime 
(Phase 1). It now carries 3 Highly Discriminative pay 
classifications at the management level. A facility under 
operational stress with documented leadership pay inequality 
is a compounding risk — low morale, high turnover, and reduced 
productivity are predictable downstream consequences.

### 5. All Negative Scores — A Directional Pattern
Every score in the dataset is negative or marginally positive. 
The algorithm is directional — negative scores indicate women 
are paid less than men. The absence of positive scores means 
there is no factory where women are compensated above parity 
in any role. The problem is systemic and unidirectional.

---

## Recommendations

| Priority | Action | Target |
|---|---|---|
| Immediate | Audit C-Level and VP compensation at Meiyo | Meiyo leadership |
| Immediate | Review Manager-tier pay across Seiko | Seiko HR |
| Short-term | Implement pay band transparency at all facilities | All factories |
| Short-term | Investigate Operational Support classifications at Meiyo (-22) | Meiyo HR |
| Medium-term | Use Berlin as internal benchmark for equitable pay structure | Global HR |
| Ongoing | Re-run equality algorithm quarterly to track remediation progress | Forensic Tech team |

---

## Deliverable
Completed classification table submitted as:
`Task_2_Equality_Table_Classified.xlsx`

Available in: `data/Task_2_Equality_Table_Classified.xlsx`