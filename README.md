# neharikab_2511729_part2_kpi_experiment


## 1. Business Context

A subscription-based digital product company launched a new onboarding and activation campaign with the objective of improving user conversion and early engagement. Users were randomly divided into two groups:

- Control group: Existing onboarding experience (n = 690)
- Treatment group: New campaign experience (n = 710)

Leadership needs to decide whether to roll out the Treatment experience to all users. The task is to frame the business problem, define the right success metrics, analyse experiment results, evaluate guardrail metrics, and deliver a data-backed launch recommendation.

**Business decision:** Should the new onboarding and activation campaign be launched to all users?


## 2. Dataset Description

File: campaign_experiment_data.xlsx (raw) → experiment_analysis.xlsx (cleaned)

| Property | Detail |

| Raw rows | 1,408 |
| Clean rows | 1,400 (8 duplicate user IDs removed) |
| Columns (raw) | 16 |
| Columns (clean) | 17 (+1 flag: is_revenue_outlier) |
| Observation window | 30 days post sign-up |

**Column reference:**

| Column | Type | Description |

| `user_id` | String | Unique user identifier |
| `signup_date` | Date | Date user signed up |
| `experiment_group` | Categorical | Control or Treatment |
| `region` | Categorical | East, West, North, South |
| `device_type` | Categorical | Mobile, Desktop, Tablet (18 missing → filled 'Unknown') |
| `traffic_source` | Categorical | Organic, Paid, Referral, Social (24 missing → filled 'Unknown') |
| `plan_type` | Categorical | Free, Basic, Premium |
| `visited_landing_page` | Binary (0/1) | Whether user visited the landing page |
| `started_trial` | Binary (0/1) | Whether user started a trial |
| `completed_onboarding` | Binary (0/1) | Whether user completed onboarding |
| `converted_to_paid` | Binary (0/1) | Whether user converted to a paid plan |
| `revenue_30d` | Numeric | Revenue generated in 30 days (USD) |
| `support_tickets_30d` | Numeric | Number of support tickets raised |
| `refund_requested` | Binary (0/1) | Whether a refund was requested |
| `days_to_convert` | Numeric | Days taken to convert (null for non-converters) |
| `engagement_score` | Numeric | Platform engagement score (14 missing → group median imputed) |
| `is_revenue_outlier` | Binary (0/1) | Flag for high-revenue outliers (added during cleaning) |

**Data quality actions taken:**

| Issue | Count | Action |

| Duplicate user IDs | 8 | Removed — first occurrence kept |
| Missing `device_type` | 18 | Filled with `'Unknown'` — rows retained |
| Missing `traffic_source` | 24 | Filled with `'Unknown'` — rows retained |
| Missing `engagement_score` | 14 | Imputed with group median — rows retained |
| Null `days_to_convert` | 1,328 | Expected — only converted users (72) have a value |
| Invalid binary values | 0 | All 5 binary columns clean |
| Revenue outliers | 9 | Flagged in `is_revenue_outlier`; retained |

Full cleaning documentation is available in the **Data Quality Log** sheet of `experiment_analysis.xlsx`.



## 3. North Star Metric Selected

**North Star: Paid Conversion Rate**
*(Users who converted to paid / total users in each group)*

**Why this metric:**
Paid conversion rate is the most direct and complete signal of campaign success. Every converted user generates immediate subscription revenue and validates that the new onboarding experience is effective. It is cleanly defined as a binary outcome per user, making it statistically well-suited for hypothesis testing.

**Why other metrics are supporting, not primary:**
- *Landing page visit rate* and *trial start rate* are upstream funnel signals — they indicate reach but not revenue impact
- *Onboarding completion rate* and *engagement score* explain the mechanism behind conversion but don't directly measure the business outcome
- *Revenue per converted user* and *days to convert* add depth to the quality of conversion but are secondary to whether conversion happens at all

**Connection to business growth:**
Higher paid conversion directly increases Monthly Recurring Revenue (MRR) and customer base size — both core drivers of subscription business growth.

**Risk of optimising blindly:**
Maximising conversion without guardrails could attract low-intent users who cancel quickly, inflate refund rates, or overwhelm the support team — all of which destroy revenue quality and customer satisfaction.



## 4. KPI Tree Summary

The North Star (Paid Conversion Rate) is driven by three primary categories, each with two sub-drivers, and is bounded by four guardrail metrics.

```
                        PAID CONVERSION RATE 
                         (North Star Metric)
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
  FUNNEL ENGAGEMENT     CONVERSION QUALITY     RETENTION HEALTH
          │                     │                     │
    ┌─────┴─────┐         ┌─────┴─────┐         ┌─────┴──────┐
    │           │         │           │         │            │
 Landing    Trial      Onboarding  Engagement  Revenue    Days to
  Page      Start      Completion   Score      per Conv.  Convert
  Visit     Rate         Rate                   User
  Rate

  ─────────────────── GUARDRAIL METRICS ───────────────────
  Refund Rate │ Support Ticket Rate │ Revenue Quality │ Segment-level Decline
```

**Primary drivers explained:**
- **Funnel Engagement** — Are users entering the conversion funnel? (landing page visits, trial starts)
- **Conversion Quality** — Are users completing the journey and becoming engaged? (onboarding completion, engagement score)
- **Retention Health** — Are converted users generating sustainable value? (revenue per converted user, days to convert)



## 5. Experiment Analysis Approach

**Files:**
- Cleaned dataset: `experiment_analysis.xlsx`
- Experiment summary & test results: `experiment_summary.xlsx`

**Approach:**

1. **Data cleaning** — Removed duplicates, imputed missing values, flagged outliers, validated binary columns. Documented in `experiment_analysis.xlsx → Data Quality Log`.

2. **Descriptive summary** — Computed Control vs Treatment values for all 11 metrics. Results in `experiment_summary.xlsx → Overall Summary`.

3. **Hypothesis testing** — Applied appropriate statistical tests to each metric:
   - Binary/proportion metrics (conversion rates, refund rate, etc.) → **Two-proportion Z-test**
   - Continuous metrics (revenue, engagement score, days to convert) → **Welch two-sample t-test**
   - All tests are **two-tailed** at **α = 0.05**

4. **Confidence intervals** — 95% CIs computed for every metric difference (Treatment − Control). CIs replace simple delta columns to communicate both direction and uncertainty of each effect.

5. **Segment analysis** — Paid conversion rate broken down by Region, Device Type, Traffic Source, and Plan Type to identify subgroup effects. Results in `experiment_summary.xlsx → Segment Analysis`.

6. **Guardrail evaluation** — Assessed refund rate, support ticket rate, engagement score, and revenue per converted user independently to check for harm. Documented in `analysis/hypothesis_test_notes.md`.



## 6. Hypothesis Test Summary

**Primary test: Paid Conversion Rate**

| Parameter | Value |

| H₀ | p_treatment − p_control = 0 (no difference) |
| H₁ | p_treatment − p_control ≠ 0 (two-tailed) |
| Test | Two-proportion Z-test |
| Significance level | α = 0.05 |
| Control conversion rate | 3.19% (22/690) |
| Treatment conversion rate | 7.04% (50/710) |
| Z-statistic | 3.264 |
| p-value | 0.0011 |
| 95% Confidence interval | [+1.56 pp, +6.15 pp] |
| **Decision** | **Reject H₀ — statistically significant** |

**Key results across all metrics:**

| Metric | Test Statistic | p-value | 95% CI | Significant? |

| Landing page visit rate | Z = 3.54 | 0.0004 | [+3.90 pp, +13.64 pp] | Yes |
| Trial start rate | Z = 1.66 | 0.0970 | [−0.71 pp, +8.59 pp] | No |
| Onboarding completion rate | Z = 2.64 | 0.0083 | [+1.43 pp, +9.52 pp] | Yes |
| **Paid conversion rate ⭐** | **Z = 3.264** | **0.0011** | **[+1.56 pp, +6.15 pp]** | **Yes** |
| Avg revenue per user | t = −0.113 | 0.9100 | [−$37.60, +$42.16] | No |
| Avg revenue per converted user | t = 2.60 | 0.0106 | [−$1,827.83, +$108.45] | Yes* |
| Refund rate | Z = −1.71 | 0.0874 | [−0.055 pp, +0.900 pp] | No |
| Support ticket rate | Z = 5.00 | < 0.001 | [+5.87 pp, +14.14 pp] | Yes |
| Avg engagement score | t = −7.88 | < 0.001 | [+4.45, +7.37] | Yes |
| Avg days to convert | t = 3.11 | 0.0036 | [−4.25, −0.68] | Yes |

*Wide CI due to only 72 total converters — interpret with caution.

Full test details and formulas: `analysis/hypothesis_test_notes.md`



## 7. Guardrail Metrics Considered

| Guardrail Metric | Control | Treatment | p-value | 95% CI | Risk Level |
| Refund rate | 0.00% | 0.42% | 0.087 | [−0.055 pp, +0.900 pp] | 🟡 Low — monitor post-launch |
| Support ticket rate | 22.0% | 37.3% | < 0.001 | [+5.87 pp, +14.14 pp] | 🔴 High — must fix before full launch |
| Avg revenue per converted user | $1,115 | $960 | 0.011 | [−$1,827.83, +$108.45] | 🟡 Watch — CI too wide to conclude |
| Avg engagement score | 57.0 | 62.9 | < 0.001 | [+4.45, +7.37] | 🟢 Positive — no risk |

**Summary of guardrail findings:**
- The only guardrail that creates a confirmed, statistically certain risk is **support ticket rate**. The CI's lower bound of +5.87 pp means even in the best-case scenario, Treatment generates meaningfully more support contacts.
- Refund rate and revenue per converted user warrant monitoring but do not block launch.
- Engagement score is a guardrail positive — higher engagement reduces churn risk.



## 8. Final Recommendation

**Decision: Launch with phased rollout**

The evidence for the Treatment is strong — paid conversion more than doubled with a tight, fully-positive 95% CI. The effect is consistent across all regions, device types, and plan types, ruling out a segment-specific artefact. Five of eleven metrics improved significantly; none of the core funnel metrics declined.

The one confirmed risk — support ticket rate — is operationally serious and must be addressed before full launch.

**Action plan:**

| Timeline | Action |

| Immediately | Launch Treatment to 25–50% of new users (phased rollout) |
| Week 1–2 | Investigate support tickets — identify which onboarding steps drive the most contacts |
| Week 2–4 | Fix top friction points; re-measure ticket rate on phased cohort |
| Week 4 | If ticket rate narrows to within 5 pp of Control → proceed to full launch |
| Month 2 | 60-day cohort analysis — track churn and LTV of Treatment converters |



## 9. Assumptions and Limitations

**Assumptions:**
- Users were randomly assigned to Control and Treatment groups (no self-selection bias)
- The 30-day observation window is representative of conversion behaviour
- First occurrence was kept for 8 duplicate user IDs — assumed to be data entry errors
- Group median imputation for 14 missing engagement scores introduces minimal bias given the small proportion (<1%)

**Limitations:**
- **Small converted user base:** Only 72 converters across both groups limits precision of all post-conversion metrics (revenue, days to convert, refund rate)
- **Short observation window:** 30-day revenue may not reflect true lifetime value — faster conversion (−2.5 days, p = 0.004) may indicate impulse decisions with higher future churn
- **Support ticket spike:** The 95% CI [+5.87 pp, +14.14 pp] rules out a small effect — full launch without a fix would significantly strain operations
- **Basic plan flat performance:** Near-zero conversion lift for Basic plan users (3.9% vs 3.6%) suggests the campaign may not be well-targeted for this segment
- **West region underperformance:** Smallest conversion lift across all regions (+1.7 pp) — possible geographic messaging or UX mismatch
- **No long-term churn data:** Cannot assess whether Treatment conversions retain at the same rate as Control conversions within this dataset



## 10. Screenshots Included
screenshots/kpi_tree_preview.png consists the KPI tree diagram showing North Star, primary drivers, sub-drivers, and guardrail metrics 
screenshots/hypothesis_test_output.png consists of the Hypothesis test results table from `experiment_summary.xlsx` 
