# Part 2 — KPI Experiment: Campaign A/B Test Analysis

> **Experiment verdict:** Conditional launch — Treatment campaign variant recommended for **Premium plan + Paid Search** segment. Full audience rollout blocked pending resolution of a support ticket spike that eliminates the net revenue advantage.

---

## Table of Contents

1. [Business Context](#1-business-context)
2. [Dataset Description](#2-dataset-description)
3. [North Star Metric](#3-north-star-metric)
4. [KPI Tree Summary](#4-kpi-tree-summary)
5. [Experiment Analysis Approach](#5-experiment-analysis-approach)
6. [Hypothesis Test Summary](#6-hypothesis-test-summary)
7. [Guardrail Metrics](#7-guardrail-metrics)
8. [Final Recommendation](#8-final-recommendation)
9. [Assumptions and Limitations](#9-assumptions-and-limitations)
10. [Repository Structure](#10-repository-structure)

---

## 1. Business Context

The business runs a SaaS product with a free-trial-to-paid subscription model. Users are acquired through multiple digital channels, guided through an onboarding experience, and then presented with a paid plan upgrade.

The existing acquisition and onboarding experience (the **Control**) had been unchanged for several cycles. The growth team designed a new campaign variant (the **Treatment**) featuring redesigned landing page creative, a restructured onboarding journey, and updated campaign targeting. Before committing to a full rollout — which would involve significant marketing budget, product engineering, and GTM resource — the team ran a controlled A/B experiment.

**The central business question:**

> Should the company replace the current user acquisition and onboarding experience with the new campaign variant across all regions, devices, and traffic channels?

This is a **go / no-go decision** with three possible outcomes:

| Outcome | Condition |
|---|---|
| Full launch | Primary metric improves, RPAU improves, all guardrails pass |
| Conditional launch | Primary metric improves but one or more guardrails are at risk |
| Do not launch | Primary metric does not improve or a critical guardrail is breached |

---

## 2. Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Period:** January 2025 – May 2025  
**Records:** 1,408 rows → **1,400 after deduplication** (8 duplicate `user_id` records removed)

### Fields

| Field | Type | Description |
|---|---|---|
| `user_id` | String | Unique user identifier |
| `signup_date` | Date | Date user signed up (2025-01-01 to 2025-05-31) |
| `experiment_group` | Categorical | `Control` or `Treatment` |
| `region` | Categorical | Geographic region: East, North, South, West |
| `device_type` | Categorical | Desktop, Mobile, Tablet |
| `traffic_source` | Categorical | Email, Organic, Paid Search, Referral, Social |
| `plan_type` | Categorical | Free, Basic, Premium |
| `visited_landing_page` | Binary (0/1) | Whether user visited the landing page |
| `started_trial` | Binary (0/1) | Whether user started a free trial |
| `completed_onboarding` | Binary (0/1) | Whether user completed the onboarding flow |
| `converted_to_paid` | Binary (0/1) | Whether user converted to a paid plan |
| `revenue_30d` | Numeric | Total revenue generated in first 30 days (USD) |
| `support_tickets_30d` | Numeric | Number of support tickets raised in 30 days |
| `refund_requested` | Binary (0/1) | Whether user requested a refund |
| `days_to_convert` | Numeric | Days from signup to paid conversion (null for non-converters) |
| `engagement_score` | Numeric | Proprietary product engagement score (0–100 scale) |

### Group Sizes

| Group | n (raw) | n (post-dedup) | Share |
|---|---|---|---|
| Control | 693 | 690 | 49.3% |
| Treatment | 715 | 710 | 50.7% |
| **Total** | **1,408** | **1,400** | — |

### Data Quality Notes

- **8 duplicate `user_id` records** removed (4 per group) — all were non-converters with $0 revenue, likely double-inserts from the data pipeline
- **`days_to_convert`** is structurally null for non-converters (94.9% null) — this is expected behaviour, not missing data
- **24 records** have missing `traffic_source`; **18** have missing `device_type`; **14** have missing `engagement_score`
- **3 Control revenue outliers** were identified: USR-100106 ($8,611), USR-100303 ($6,789), USR-100103 ($3,888) — these significantly inflate the Control average revenue per converter and require verification

---

## 3. North Star Metric

### Selected: Revenue Per Acquired User (RPAU) — 30 Days

```
RPAU = Total 30-Day Revenue ÷ Total Users in Group
```

RPAU was selected because it integrates the two variables that determine the business case simultaneously:

- **Conversion volume** — how many users became paying customers
- **Revenue quality** — how much each conversion was worth

Unlike alternatives, RPAU captures the full picture:

| Metric | Why it was not chosen as NSM |
|---|---|
| Paid conversion rate | Ignores deal size — high conversion at low revenue is a growth illusion |
| Avg revenue per converter | Distorted by outliers; excludes the 93% of users who did not convert |
| Total revenue | Reflects group size, not per-user efficiency |
| Engagement score | Predictive but produces no direct revenue signal |

### RPAU Result

| | Control | Treatment | Lift | p-value |
|---|---|---|---|---|
| Gross RPAU | $51.97 | $54.25 | +4.4% | 0.0013 ★★★ |
| Net RPAU (after support cost) | $48.67 | $48.66 | −$0.01 | — |

> ⚠️ The gross RPAU advantage (+$2.28/user) is eliminated when support costs are factored in (+$2.29/user at estimated $15/ticket).

---

## 4. KPI Tree Summary

```
RPAU — Revenue Per Acquired User (North Star)
│
├── Driver 1: Conversion Engine
│   ├── Landing Page Visit Rate        63.6% → 72.4%  (+13.8%)  ★★★
│   ├── Trial Start Rate               25.1% → 29.0%  (+15.7%)  ns
│   ├── Onboarding Completion Rate     15.6% → 21.1%  (+35.0%)  ★★
│   └── Paid Conversion Rate           3.19% → 7.04%  (+120.9%) ★★★ ← PRIMARY
│
├── Driver 2: Revenue Quality
│   ├── Avg Revenue / Converter        $1,630 → $770  (−52.7%)  ns (outlier-driven)
│   └── Plan Mix                       Broadly balanced across groups
│
├── Driver 3: Retention & Stickiness
│   ├── Avg Engagement Score           57.0 → 62.9    (+10.4%)  ★★★
│   └── Avg Days to Convert            8.9 → 6.4 days (−27.8%)  ★★
│
└── Guardrails
    ├── Support Ticket Rate            14.8% → 24.8%  (+67.7%)  ★★★ ← BREACHED
    ├── Refund Rate                    0.00% → 0.42%  (new)     ns  ← WATCH
    └── Engagement Score               57.0 → 62.9    (+10.4%)  ★★★ ← PASSED
```

**Key insight:** The Driver 2 gap (avg revenue per converter) is not statistically significant (p=0.064) and is entirely explained by three Control outliers. Removing them drops the Control average to ~$640 — below Treatment. The RPAU level, which includes all 1,400 users, is the most reliable comparison metric.

---

## 5. Experiment Analysis Approach

### Design

- **Type:** Two-group randomised controlled experiment (A/B test)
- **Unit of randomisation:** Individual user (`user_id`)
- **Assignment:** Random allocation at signup
- **Observation window:** 30 days post-signup
- **Significance level:** α = 0.05 (two-tailed)
- **Required power:** 80% (1−β = 0.80)

### Statistical Tests Used

| Metric type | Test | Rationale |
|---|---|---|
| Binary outcomes (rates) | Two-proportion z-test | Large n, binary outcome, np ≥ 5 in both groups — CLT applies |
| Revenue (RPAU, per-converter) | Mann-Whitney U | Right-skewed distribution with outliers; non-parametric preferred over t-test |
| Engagement score | Welch's t-test | Continuous score; unequal variance not assumed |
| Refund rate | Fisher Exact test | Control has zero events; z-test invalid when a cell is zero |

### Power Analysis

| Parameter | Value |
|---|---|
| Baseline conversion rate (Control) | 3.19% |
| Min Detectable Effect (MDE) | 2.65pp at 80% power |
| Observed effect | 3.85pp (+120.9% relative) |
| Observed effect vs MDE | 1.45× above MDE |
| Required n per group | 516 |
| Actual n (Control / Treatment) | 690 / 710 ✓ |
| Observed statistical power | **90.4%** ✓ |

The experiment was adequately powered. The observed effect is 1.45× larger than the minimum detectable effect, and the actual sample sizes exceed the required minimum for this effect size at 80% power.

### Segment Analysis

Four dimensions were analysed for treatment effect heterogeneity:

| Dimension | Values |
|---|---|
| Region | East, North, South, West |
| Device type | Desktop, Mobile, Tablet |
| Traffic source | Email, Organic, Paid Search, Referral, Social |
| Plan type | Free, Basic, Premium |

---

## 6. Hypothesis Test Summary

### H1 — Paid Conversion Rate (Primary Metric)

| | |
|---|---|
| **H₀** | Paid conversion rate is equal across groups (p_C = p_T) |
| **H₁** | Paid conversion rate differs between groups (p_C ≠ p_T) |
| **Test** | Two-proportion z-test, two-tailed, α = 0.05 |
| **Result** | z = 3.264, **p = 0.0011** |
| **95% CI** | [+1.54pp, +6.17pp] — excludes zero |
| **Decision** | **REJECT H₀** |
| **Interpretation** | Treatment conversion rate (7.04%) is significantly higher than Control (3.19%). The +3.85pp absolute lift is statistically confirmed and not due to random variation. |

### H2 — RPAU (North Star Metric)

| | |
|---|---|
| **H₀** | Average 30-day RPAU is equal across groups (μ_C = μ_T) |
| **H₁** | Average 30-day RPAU differs between groups (μ_C ≠ μ_T) |
| **Test** | Mann-Whitney U (revenue is right-skewed; 93% of users have $0 revenue) |
| **Result** | **p = 0.0013** |
| **Decision** | **REJECT H₀** |
| **Interpretation** | Gross RPAU is significantly higher in Treatment ($54.25 vs $51.97). However, net RPAU after support cost is effectively identical ($48.66 vs $48.67). |

### H3 — Support Ticket Rate (Guardrail)

| | |
|---|---|
| **H₀** | Support ticket rate is equal across groups (p_C = p_T) |
| **H₁** | Support ticket rate differs between groups (p_C ≠ p_T) |
| **Test** | Two-proportion z-test, two-tailed, α = 0.05 |
| **Result** | z = 4.692, **p < 0.001** |
| **Decision** | **REJECT H₀ — Guardrail BREACHED** |
| **Interpretation** | Treatment users raise support tickets at 67.7% higher rates (24.8% vs 14.8%). This is universal across all segments and eliminates the gross RPAU advantage when support costs are factored in. |

### All Metrics at a Glance

| Metric | Control | Treatment | Lift | p-value | Decision |
|---|---|---|---|---|---|
| Landing page visit rate | 63.6% | 72.4% | +13.8% | <0.001 ★★★ | Reject H₀ |
| Trial start rate | 25.1% | 29.0% | +15.7% | 0.097 ns | Fail to reject |
| Onboarding completion | 15.6% | 21.1% | +35.0% | 0.008 ★★ | Reject H₀ |
| **Paid conversion rate** | **3.19%** | **7.04%** | **+120.9%** | **0.001 ★★★** | **Reject H₀** |
| **RPAU (NSM)** | **$51.97** | **$54.25** | **+4.4%** | **0.001 ★★★** | **Reject H₀** |
| Avg rev / converter | $1,630 | $770 | −52.7% | 0.064 ns | Fail to reject |
| Engagement score | 57.0 | 62.9 | +10.4% | <0.001 ★★★ | Reject H₀ |
| Days to convert | 8.9 | 6.4 | −27.8% | 0.009 ★★ | Reject H₀ |
| **Support ticket rate** | **14.8%** | **24.8%** | **+67.7%** | **<0.001 ★★★** | **Reject H₀ ⚠** |
| Refund rate | 0.00% | 0.42% | New signal | 0.250 ns | Fail to reject |

> ★★★ p < 0.001 &nbsp; ★★ p < 0.01 &nbsp; ★ p < 0.05 &nbsp; ns = not significant

---

## 7. Guardrail Metrics

Guardrail metrics are boundaries that must not be violated even when primary metrics improve. A significant worsening in any guardrail overrides a positive primary result and blocks or restricts rollout.

### Guardrail 1 — Support Ticket Rate

| | |
|---|---|
| **Threshold** | ≤ Control level (14.8%) |
| **Control** | 14.8% (102 of 690 users) |
| **Treatment** | 24.8% (176 of 710 users) |
| **Lift** | +67.7% relative / +10.0pp absolute |
| **p-value** | < 0.001 ★★★ |
| **Status** | 🔴 **BREACHED** |

**Financial consequence:** At an estimated $15/ticket operational cost, Treatment adds $2.29/user in support cost ($5.60 vs $3.30). This exactly cancels the gross RPAU gain of +$2.28/user. Net RPAU after support cost: Control $48.67 vs Treatment $48.66.

**Segment breakdown (support ticket lift):**

| Segment | Control | Treatment | Lift |
|---|---|---|---|
| Email traffic | 16.4% | 33.9% | +106% |
| Referral traffic | 12.3% | 28.6% | +131% |
| Free plan | 12.2% | 24.0% | +97% |
| Tablet device | 10.9% | 21.4% | +96% |
| Paid Search | 18.7% | 22.2% | +18% ← mildest |
| Premium plan | 23.9% | 26.8% | +12% ← mildest |

The spike is concentrated in first-time contacts (1-ticket users: 9.3% → 16.1%), indicating a **single friction point** in the Treatment onboarding flow rather than a general product quality issue.

### Guardrail 2 — Refund Rate

| | |
|---|---|
| **Threshold** | ≤ 0.50% (hard cap) |
| **Control** | 0.00% (0 refunds) |
| **Treatment** | 0.42% (3 refunds) |
| **p-value** | 0.250 ns (Fisher Exact — Control cell = 0) |
| **Status** | 🟡 **WATCH — 0.08pp below cap** |

Three structural observations despite statistical inconclusion:
- All 3 Treatment refunds came from **mobile users**
- 2 of 3 are in the **West region**
- Among converters only: 6.0% refund rate (3/50) in Treatment vs 0.0% (0/22) in Control

At 10× scale, a 6% converter refund rate → ~30 events, which would be highly significant.

### Guardrail 3 — Engagement Score

| | |
|---|---|
| **Threshold** | ≥ Control level (57.0) |
| **Control** | 57.03 (mean) |
| **Treatment** | 62.94 (mean) |
| **Lift** | +10.4% |
| **p-value** | < 0.001 ★★★ (t = 7.93) |
| **Status** | 🟢 **PASSED** |

Significant across **15 of 16** segments. The Very High band (score 80+) more than doubled: 5.6% → 12.4%. The entire score distribution shifted upward — improvement is not driven by a small subset of outlier users.

> **Important:** Engagement and support tickets are independent signals. High-engagement Treatment users still raise tickets at 23.2% vs 12.8% for equivalently engaged Control users. These require separate interventions.

---

## 8. Final Recommendation

### Decision: Conditional Launch

> **Launch to Premium plan + Paid Search traffic only. Do not launch to full audience until the support ticket issue is resolved.**

### Rationale

**For launching now (selected segment):**
- Premium plan Treatment users: +127% conversion, +86.7% RPAU ($100.56 vs $53.86), only +12.3% support ticket lift, +15.7% engagement
- Paid Search Treatment users: +384% conversion, +219% RPAU, only +18.4% support ticket lift
- Both cohorts show positive net economics and are safe to launch immediately

**Against full rollout:**
- At 10× scale (14,000 users): gross revenue advantage ≈ +$31,900; additional support cost ≈ +$32,100; **net result ≈ −$197**
- All 3 refunds are from mobile users — mobile-inclusive rollout risks breaching the 0.50% refund cap

### Segment Assessment

| Segment | Conversion Lift | RPAU Lift | Support Lift | Recommendation |
|---|---|---|---|---|
| Premium plan | +127% | +87% | +12% | ✅ Launch |
| Paid Search | +384% | +219% | +18% | ✅ Launch |
| Basic plan | +7% | — | +73% | ⏸ Hold |
| Free plan | +204% | +125% | +97% | ⏸ Hold |
| Referral | +345% | High | +131% | ⏸ Hold |
| Email | +161% | Strong | +106% | ⏸ Hold |
| Tablet | +293% | Strong | +96% | ⏸ Hold |
| Mobile device | +187% | Positive | +65% | ⚠ Caution |

### Decision Gates Before Broader Rollout

| Gate | Condition | Target | Timeline |
|---|---|---|---|
| Gate 1 | Support ticket rate in launched segment | ≤ 16% | Week 4 |
| Gate 2 | Refund rate in launched segment | < 0.45% | Week 4 |
| Gate 3 | 30→60 day paid retention vs Control | ≥ Control rate | Week 6 |
| Gate 4 | Fixed Treatment ticket rate (post-onboarding fix) | ≤ 14.8% | Week 8 |

---

## 9. Assumptions and Limitations

### Assumptions

| # | Assumption | Basis |
|---|---|---|
| A1 | Support cost of **$15 per ticket** | Industry estimate; not confirmed with operations team |
| A2 | Deduplication by keeping first `user_id` occurrence is valid | All duplicates were non-converters with $0 revenue |
| A3 | `days_to_convert` being null for non-converters is structural, not missing data | Confirmed: every converter has a value, every non-converter is null |
| A4 | The 30-day observation window is sufficient to capture primary conversion behaviour | Standard for SaaS free-trial experiments |
| A5 | The experiment was properly randomised | Balance check shows 1.6% group size imbalance — within ±5% tolerance |
| A6 | Engagement score ≥ 80 constitutes "Very High" engagement | Based on observed score distribution |

### Limitations

| # | Limitation | Severity | Mitigation |
|---|---|---|---|
| L1 | **No 30→60 day retention data.** LTV, churn, and plan upgrades are unmeasured. Engagement score is a leading indicator only. | Medium | Collect 60-day data on launched cohort before Gate 3 decision |
| L2 | **$15/ticket support cost is an estimate.** If true cost is $20+, net RPAU worsens further. | Medium | Confirm real cost with operations team before finalising launch economics |
| L3 | **Three Control revenue outliers** (max $8,611) inflate Control avg revenue per converter from ~$640 to $1,630. Billing accuracy unverified. | Medium | Audit USR-100106, USR-100303, USR-100103 for billing accuracy |
| L4 | **Refund rate statistically inconclusive** (p=0.250, n=3 events). Sample too small to confirm systematic pattern. | High (at scale) | Set automated alert at 0.45%; monitor refund cohort composition |
| L5 | **Minor segment allocation imbalances.** North is +4.1pp in Control; West is +3.6pp in Treatment. Email is −2.8pp in Treatment. | Low | Apply stratification in follow-up experiment |
| L6 | **No multi-device or multi-session tracking.** A user who visits on mobile and converts on desktop would be attributed to their signup device only. | Low-Medium | Flag as known gap in attribution methodology |
| L7 | **Novelty effect not controlled.** The experiment may capture inflated early engagement from users responding to the "newness" of the Treatment. | Low | Compare early vs late experiment cohorts in follow-up analysis |

---

## 10. Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx       # Raw experiment dataset
│
├── analysis/
│   ├── experiment_analysis.xlsx            # Full metric-by-metric analysis
│   └── hypothesis_test_notes.md           # Statistical test methodology notes
│
├── outputs/
│   ├── kpi_tree.png                        # KPI tree visualisation
│   ├── experiment_summary.xlsx            # Control vs Treatment summary table
│   └── recommendation_memo.pdf            # Full recommendation memo
│
├── screenshots/
│   ├── summary_metrics.png                # Dashboard screenshots
│   ├── hypothesis_test_output.png         # Test output screenshots
│   └── kpi_tree_preview.png              # KPI tree preview
│
└── README.md                              # This file
```

---

## Quick Reference

```
Experiment period : January 2025 – May 2025
Total users       : 1,400 (post-dedup)
Control           : 690 users
Treatment         : 710 users
Primary metric    : Paid Conversion Rate
North Star        : RPAU — Revenue Per Acquired User (30-day)
α (significance)  : 0.05 (two-tailed)
Observed power    : 90.4%
Verdict           : Conditional Launch — Premium + Paid Search only
```

---

*Prepared by the Business Analysis Team — June 2026*
