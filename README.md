# SwiftCart Supply Chain Optimization
## A Full Six Sigma DMAIC Project | Statistical Analysis Using JASP

![Six Sigma](https://img.shields.io/badge/Methodology-Six%20Sigma%20DMAIC-blue)
![Tool](https://img.shields.io/badge/Tool-JASP%20(Open%20Source)-orange)
![Status](https://img.shields.io/badge/Status-Complete-green)
![Domain](https://img.shields.io/badge/Domain-Supply%20Chain-purple)

---

## 📌 Table of Contents
1. [Project Overview](#project-overview)
2. [Business Context](#business-context)
3. [DMAIC Framework](#dmaic-framework)
4. [Phase 1 — Define](#phase-1--define)
5. [Phase 2 — Measure](#phase-2--measure)
6. [Phase 3 — Analyse](#phase-3--analyse)
7. [Phase 4 — Improve](#phase-4--improve)
8. [Phase 5 — Control](#phase-5--control)
9. [Key Findings Summary](#key-findings-summary)
10. [Tools & Techniques Used](#tools--techniques-used)
11. [Dataset](#dataset)
12. [How to Reproduce This Analysis](#how-to-reproduce-this-analysis)
13. [About the Author](#about-the-author)

---

## Project Overview

This project applies the **Six Sigma DMAIC methodology** to a real-world supply chain problem — identifying and quantifying fulfillment time variability across three third-party suppliers for a simulated e-commerce company called **SwiftCart**.

Using **JASP** (an open-source statistical software comparable to Minitab), I ran a full statistical investigation including descriptive analysis, hypothesis testing, post-hoc comparisons, statistical process control charts, and process capability analysis.

The result: one supplier was found to be generating an estimated **533,333 defects per million orders** — while appearing stable on the surface. Six Sigma tools exposed what basic reporting never would.

---

## Business Context

> *"SwiftCart's customer complaints about late deliveries increased by 23% in Q3 2024. The Operations Manager commissioned a process improvement investigation to identify whether the issue was systemic and which suppliers were responsible."*

**Company:** SwiftCart (simulated e-commerce retailer)
**Analyst Role:** Process Improvement Analyst
**Suppliers Under Review:** Supplier A, Supplier B, Supplier C
**Orders Analysed:** 90 (30 per supplier)
**Metric:** Order Fulfillment Time (days from order placement to delivery)

---

## DMAIC Framework

DMAIC is the core problem-solving framework of Six Sigma:

| Phase | Purpose | Key Output |
|---|---|---|
| **Define** | Clarify the problem and scope | Problem Statement, SIPOC, CTQ |
| **Measure** | Quantify current performance | Descriptive Stats, Normality Tests |
| **Analyse** | Identify root causes statistically | ANOVA, Tukey Post-hoc, Levene's Test |
| **Improve** | Recommend data-driven solutions | Fishbone, Impact/Effort Matrix |
| **Control** | Sustain improvements over time | SPC Charts, Cpk, Supplier Scorecard |

---

## Phase 1 — Define

### Problem Statement

> *"Order fulfillment times at SwiftCart have shown increasing variability across three suppliers (A, B, C) since Q3 2024, contributing to a 23% rise in late delivery complaints. The goal of this project is to identify which supplier(s) drive this variability and reduce average fulfillment time to under 5 days with consistent performance across all suppliers."*

### CTQ — Critical to Quality Metric

| CTQ Metric | Target | Upper Spec Limit (USL) |
|---|---|---|
| Order Fulfillment Time | ≤ 5 days | 7 days |

**Defect Definition:** Any order where fulfillment time exceeds 5 days (the CTQ target).

### SIPOC Diagram

| Suppliers | Inputs | Process | Outputs | Customers |
|---|---|---|---|---|
| Supplier A, B, C | Purchase Orders | Order Receipt → Pick & Pack → Dispatch → Delivery | Fulfilled Orders | SwiftCart Customers |
| WMS System | Inventory Data | ↓ | Fulfillment Time (days) | Operations Manager |
| Logistics Partners | Shipping Labels | ↓ | Delivery Confirmation | Customer Service Team |

### Key Hypothesis Going Into Measure Phase

Given that fulfillment time is a cumulative metric across multiple process steps (receipt, pick & pack, dispatch, delivery), variability was expected to enter at multiple stages — with a **domino effect** propagating delay from one step to the next. This is known as **propagated variation** in Six Sigma.

---

## Phase 2 — Measure

### Descriptive Statistics

| Metric | Supplier A | Supplier B | Supplier C |
|---|---|---|---|
| **N** | 30 | 30 | 30 |
| **Mean (days)** | 4.450 | **7.163** | 5.557 |
| **Std. Deviation** | 0.535 | 0.736 | **0.795** |
| **Minimum** | 3.500 | 5.900 | 4.200 |
| **Maximum** | 5.300 | 8.400 | 6.900 |

**Initial Observations:**
- **Supplier A** performs well within the CTQ target with low variability — a stable, capable process
- **Supplier B** has a mean of 7.16 days — already exceeding the USL of 7 days on average
- **Supplier C** has the highest standard deviation (0.795) — an inconsistent, unpredictable process that frequently breaches the 5-day CTQ target despite a seemingly acceptable mean

### Normality Testing — Shapiro-Wilk

Before running inferential statistics, normality was verified for each supplier group:

| Supplier | W Statistic | p-value | Result |
|---|---|---|---|
| Supplier A | 0.960 | 0.303 | ✅ Normal |
| Supplier B | 0.964 | 0.385 | ✅ Normal |
| Supplier C | 0.966 | 0.442 | ✅ Normal |

All p-values > 0.05 → normality assumption satisfied → ANOVA is valid to proceed.

---

## Phase 3 — Analyse

### ANOVA Assumption Check — Levene's Test

Before running ANOVA, equal variance across groups was verified:

| F | df1 | df2 | p-value | Result |
|---|---|---|---|---|
| 2.668 | 2 | 87 | 0.075 | ✅ Equal variances assumed |

p = 0.075 > 0.05 → Levene's test passed → standard ANOVA is appropriate.

### One-Way ANOVA Results

**Hypotheses:**
- H₀: Mean fulfillment time is equal across all three suppliers
- H₁: At least one supplier has a significantly different mean fulfillment time

| Source | Sum of Squares | df | Mean Square | F | p |
|---|---|---|---|---|---|
| **Supplier** | 111.7 | 2 | 55.84 | **114.7** | **< .001** |
| Residuals | 42.36 | 87 | 0.487 | | |

**Result:** F = 114.7, p < .001 → **Reject H₀**

The variation between suppliers is 114.7 times larger than variation within each supplier's own orders. Supplier identity is a massive driver of fulfillment time variability.

### Tukey Post-Hoc Test (HSD)

ANOVA confirms a significant difference exists — Tukey identifies exactly which pairs differ:

| Comparison | Mean Difference | p-value (Tukey) | Significant? |
|---|---|---|---|
| **A vs B** | -2.713 days | < .001 | ✅ YES |
| **A vs C** | -1.107 days | < .001 | ✅ YES |
| **B vs C** | +1.607 days | < .001 | ✅ YES |

**Every single supplier pair is significantly different from each other.**

### Supplier Verdict

| Supplier | Mean | Verdict | Reason |
|---|---|---|---|
| **Supplier A** | 4.45 days | ✅ Benchmark | Low mean + Low variation. Best-in-class. |
| **Supplier C** | 5.56 days | ⚠️ At Risk | High variation — unpredictable despite acceptable mean |
| **Supplier B** | 7.16 days | ❌ Critical Failure | Mean exceeds USL. Immediate intervention required. |

---

## Phase 4 — Improve

### Root Cause Analysis — Fishbone (Ishikawa) Diagram

Before prescribing solutions, a structured root cause analysis was conducted across the 6Ms framework:

| Category | Possible Root Causes (Supplier B) |
|---|---|
| **Machine** | Outdated warehouse management system, no automation |
| **Method** | No standardised pick & pack SOPs, inefficient workflows |
| **Material** | Stockouts, poor inventory replenishment planning |
| **Man** | Undertrained staff, high turnover, labour shortages |
| **Measurement** | No real-time order tracking, SLAs not monitored |
| **Mother Nature** | Geographic distance, geopolitical delays, customs bottlenecks |

> **Key Insight:** The investigation prioritised geographic, geopolitical and operational factors before prescribing solutions. A common mistake in process improvement is jumping to solutions before understanding root causes — this project followed the Six Sigma principle of *diagnose first, prescribe second.*

### Recommended Improvements

**For Supplier B (Critical Priority 🔴):**
- Conduct full operational audit of Supplier B's pick & pack process
- Investigate geographic and logistics route efficiency
- Implement automated inventory reorder triggers
- Introduce weekly SLA performance review meetings
- Set contractual penalty clauses for orders breaching USL of 7 days
- Initiate dual sourcing — redirect % of Supplier B volume to Supplier A while improvements are made

**For Supplier C (Medium Priority 🟡):**
- Conduct process mapping to identify variability sources
- Implement Standard Operating Procedures (SOPs) for fulfillment
- Deploy SPC monitoring to detect early warning signals

**For Supplier A (Maintain & Replicate 🟢):**
- Document Supplier A's process as the internal benchmark
- Share best practices with Suppliers B and C
- Reward with increased order volume allocation as incentive

### Impact vs Effort Matrix

| Solution | Impact | Effort | Priority |
|---|---|---|---|
| Automated reorder system (B) | High | Medium | ⭐ Do First |
| SLA penalty clauses (B) | High | Low | ⭐ Do First |
| SOPs for Supplier C | Medium | Low | ✅ Do Next |
| Dual sourcing strategy | High | High | 📅 Plan for Month 3 |
| Best practice sharing (A→B,C) | Medium | Low | ✅ Do Next |
| Alternative supplier qualification | High | High | 📅 Plan for Month 2 |

---

## Phase 5 — Control

### Statistical Process Control — I-MR Charts

SPC charts were built for Supplier A (to protect baseline performance) and Supplier B (to monitor improvement progress).

**Supplier A — I-MR Chart Results:**
- Individuals Chart: ✅ No violations — process is stable and in control
- Moving Range Chart: ⚠️ Point 29 flagged — upward trend detected (Orders 27→28→29 increased by ~0.8 days per order)
- **Action:** Monitor next 5-10 orders closely. Do not overreact — one trend signal does not confirm a permanent shift. Investigate what changed around Order 27.

**Supplier B — I-MR Chart Results:**
- Individuals Chart: ✅ No violations — but this is expected
- Moving Range Chart: ⚠️ Point 9 flagged — shift detected
- **Key Insight:** Supplier B's process is *in control but incapable*. Its control limits are set by its own poor historical performance — meaning the process is consistently, predictably failing. This is more dangerous than an erratic process because it can run indefinitely without triggering alarms unless specification limits are actively monitored.

> **Critical Six Sigma Distinction:**
> - *In Control* = the process is behaving consistently with its own history
> - *Capable* = the process can actually meet the customer's requirement
> - **A process can be in control AND incapable at the same time — exactly the situation with Supplier B.**

### Process Capability Analysis (Cpk)

| Metric | Supplier A | Supplier B |
|---|---|---|
| **Cpk** | **1.381** ✅ | **-0.064** ❌ |
| **Ppk** | 1.587 | -0.074 |
| **ppm Observed** | **0** | **533,333** |
| **ppm Expected** | 0.957 | 587,810 |
| **Verdict** | Capable | Catastrophically Incapable |

**Cpk Interpretation:**
- Cpk ≥ 1.33 = Capable process (Six Sigma standard) ✅
- Cpk 1.00–1.33 = Marginally capable ⚠️
- Cpk < 1.00 = Incapable ❌
- **Cpk < 0 = Process mean has crossed the specification limit entirely** 🚨

Supplier B's **negative Cpk** confirms its mean (7.16 days) has completely exceeded the USL (7 days). This is not a process that occasionally fails — it is a process that has structurally failed.

### Supplier Scorecard (Ongoing Control)

| KPI | Target | Warning Zone | Critical Zone |
|---|---|---|---|
| Mean Fulfillment Time | ≤ 5 days | 5–6 days | > 6 days |
| % Orders On Time | ≥ 95% | 85–95% | < 85% |
| Std Deviation | ≤ 0.6 | 0.6–0.8 | > 0.8 |
| Cpk | ≥ 1.33 | 1.00–1.33 | < 1.00 |

### Tiered Incentive Model

| Performance Tier | Mean Fulfillment | Reward |
|---|---|---|
| 🥇 Gold | ≤ 4.5 days | +15% order volume + early payment terms |
| 🥈 Silver | 4.5–5.0 days | +5% order volume |
| 🥉 Bronze | 5.0–6.0 days | Standard terms + improvement plan required |
| ❌ At Risk | > 6.0 days | Performance review + sourcing alternatives activated |

### Escalation Response Plan

| Trigger | Action | Owner |
|---|---|---|
| Single order > 7 days | Flag and investigate | Operations Analyst |
| Monthly mean > 6 days | Formal supplier review meeting | Supply Chain Manager |
| 2 consecutive months > 6 days | Activate dual sourcing | Procurement Director |
| 3 consecutive months > 6 days | Terminate supplier contract | C-Suite Decision |

---

## Key Findings Summary

| Finding | Detail |
|---|---|
| **ANOVA Result** | F = 114.7, p < .001 — supplier identity explains the vast majority of fulfillment variability |
| **Most Capable Supplier** | Supplier A — Cpk 1.381, zero observed defects, mean 4.45 days |
| **Most Inconsistent Supplier** | Supplier C — highest SD (0.795), unpredictable performance |
| **Critical Failure** | Supplier B — Cpk -0.064, mean 7.16 days exceeding USL, 533,333 ppm defect rate |
| **Root Cause** | Geographic, geopolitical and operational factors driving Supplier B delays |
| **Primary Recommendation** | Dual sourcing + supplier incentive model + real-time SPC monitoring |

---

## Tools & Techniques Used

| Category | Tool / Technique |
|---|---|
| **Statistical Software** | JASP (Open Source) |
| **Methodology** | Six Sigma DMAIC |
| **Descriptive Analysis** | Mean, Std Dev, Min/Max by group |
| **Normality Testing** | Shapiro-Wilk Test |
| **Equal Variance Testing** | Levene's Test |
| **Hypothesis Testing** | One-Way ANOVA |
| **Post-Hoc Analysis** | Tukey HSD |
| **Process Monitoring** | I-MR Control Charts (Western Electric Rules) |
| **Capability Analysis** | Cpk, Ppk, ppm analysis |
| **Root Cause Analysis** | Fishbone (Ishikawa) Diagram — 6Ms Framework |
| **Prioritisation** | Impact vs Effort Matrix |
| **Control Planning** | Supplier Scorecard + Tiered Incentive Model |

---

## Dataset

The dataset contains 90 simulated orders across 3 suppliers — 30 per supplier — with fulfillment times recorded in days.

📁 `/data/suppliers.csv` — Full 90-order dataset
📁 `/data/SupplierA_SPC.csv` — Supplier A sequential data for SPC analysis
📁 `/data/SupplierB_SPC.csv` — Supplier B sequential data for SPC analysis

**Dataset Structure:**

```
Order_ID, Supplier, Fulfillment_Days
1, Supplier_A, 4.2
2, Supplier_A, 3.8
...
31, Supplier_B, 6.8
...
61, Supplier_C, 5.1
```

---

## How to Reproduce This Analysis

1. **Download JASP** (free) from [https://jasp-stats.org](https://jasp-stats.org)
2. **Clone this repository** and navigate to the `/data` folder
3. **Load `suppliers.csv`** into JASP
4. **Run Descriptive Statistics:**
   - Descriptives → Move `Fulfillment_Days` to Variables, `Supplier` to Split
   - Enable: Mean, Std Dev, Min, Max, Shapiro-Wilk
5. **Run ANOVA:**
   - ANOVA → Classical → ANOVA
   - Dependent Variable: `Fulfillment_Days` | Fixed Factor: `Supplier`
   - Enable: Assumption Checks (Levene's + Normality)
   - Enable: Post Hoc Tests → Tukey
6. **Run SPC Charts:**
   - Control Charts → Variables Charts for Individuals
   - Load `SupplierA_SPC.csv` and `SupplierB_SPC.csv` separately
   - Measurements: `Fulfillment_Days`
7. **Run Capability Analysis:**
   - Capability Analysis → Process Capability Study
   - Measurements: `Fulfillment_Days`
   - USL: 7 | LSL: 0 | Target: 5

---

## About the Author

**[Jui Mathuria]**
Six Sigma Green Belt Candidate | Supply Chain & Process Improvement

I built this project as part of my Six Sigma Green Belt learning journey to develop hands-on experience with statistical process improvement tools. The analysis was conducted entirely in JASP — a free, open-source alternative to Minitab — demonstrating that rigorous Six Sigma analysis is accessible without expensive software licences.

📧 [jui.mathuria.2025@marshall.usc.edu]
💼 [www.linkedin.com/in/juimathuria]

---

> *"The most dangerous supplier isn't the unpredictable one — it's the one that's consistently and predictably failing. Six Sigma doesn't just find problems. It proves them, quantifies them, and gives you the language to act on them."*

---

![DMAIC](https://img.shields.io/badge/Define-✓-blue)
![DMAIC](https://img.shields.io/badge/Measure-✓-blue)
![DMAIC](https://img.shields.io/badge/Analyse-✓-blue)
![DMAIC](https://img.shields.io/badge/Improve-✓-blue)
![DMAIC](https://img.shields.io/badge/Control-✓-blue)
