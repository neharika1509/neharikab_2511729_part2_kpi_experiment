# Recommendation Memo: Campaign Experiment Launch Decision



## 1. Executive Summary
The Treatment (new onboarding and activation campaign) has more than doubled paid conversion rate compared to the Control group — from 3.19% to 7.04% — a statistically significant result (Z = 3.264, p = 0.0011, 95% CI: [+1.56 pp, +6.15 pp]). The entire confidence interval lies above zero, confirming the direction and minimum magnitude of the lift with 95% confidence. Engagement scores and funnel metrics also improved significantly.

However, the Treatment group shows a statistically significant spike in support ticket volume (+15.3 pp, p < 0.001, 95% CI: [+5.87 pp, +14.14 pp]), which shows a material operational risk. Additionally, average revenue per converted user declined (Treatment: $960 vs Control: $1,115), a result that reached statistical significance (t = 2.60, p = 0.011), though its confidence interval is very wide due to the small converted user base.

Recommendation: Launch with a phased rollout and parallel support investment. The conversion gains are large, statistically robust, and consistent across all segments. However, a full immediate launch without resolving the support ticket issue would be operationally unsustainable.



## 2. North Star Metric

Selected North Star: Paid Conversion Rate

- This is the most direct measure of campaign success — every converted user generates subscription revenue and validates the new onboarding experience.
- It connects paid subscription growth to long-term business revenue, making it the clearest signal for the launch decision.
- Other metrics such as engagement score, onboarding completion rate are supporting indicators — they explain why conversion changes but are not the primary success criteria.
- Risk of blind optimisation: If conversion is maximised without guardrails, the campaign could attract users who cancel quickly, inflate refund rates, or overwhelm support — destroying revenue quality. This is why guardrail metrics are essential.



## 3. KPI Tree Explanation

The North Star (Paid Conversion Rate) breaks down into three primary driver categories:

Funnel Engagement — Captures whether users enter the conversion funnel at all. 
Sub-drivers: Landing Page Visit Rate and Trial Start Rate. 
- Landing page visit rate improved significantly (Z = 3.54, p = 0.0004, 95% CI: [+3.90 pp, +13.64 pp]). 
- Trial start rate trended positive but did not reach significance (p = 0.097, 95% CI: [−0.71 pp, +8.59 pp]).

Conversion Quality — Captures how effectively users complete the onboarding journey and become engaged. 
Sub-drivers: Onboarding Completion Rate and Engagement Score. Both improved significantly 
— onboarding completion (Z = 2.64, p = 0.0083, 95% CI: [+1.43 pp, +9.52 pp]) 
- engagement score (t = −7.88, p < 0.001, 95% CI: [+4.45, +7.37 points]).

Retention Health — Captures value delivered after conversion. 
Sub-drivers: Revenue per Converted User and Days to Convert. 
- Revenue per converted user declined (Treatment: $960 vs Control: $1,115; t = 2.60, p = 0.011), though the CI is very wide ([−$1,827.83, +$108.45]) given only 72 converters. 
- Days to convert improved significantly (t = 3.11, p = 0.0036, 95% CI: [−4.25, −0.68 days]).

Guardrail metrics - Refund Rate, Support Ticket Rate, Revenue Quality, Segment-level Decline. They act as checks to ensure the North Star improvement is sustainable.



## 4. Experiment Result Summary

All tests are two-tailed at α = 0.05. Proportions use two-proportion Z-tests; continuous metrics use Welch two-sample t-tests. Confidence intervals are at the 95% level.

| Metric | Control | Treatment | 95% CI (Treatment − Control) | Test Statistic | p-value | Significant? |
|---|---|---|---|---|---|---|
| Users | 690 | 710 | — | — | — | — |
| Landing page visit rate | 63.6% | 72.4% | [+3.90 pp, +13.64 pp] | Z = 3.54 | 0.0004 | Yes |
| Trial start rate | 25.1% | 29.0% | [−0.71 pp, +8.59 pp] | Z = 1.66 | 0.0970 | No |
| Onboarding completion rate | 15.7% | 21.1% | [+1.43 pp, +9.52 pp] | Z = 2.64 | 0.0083 | Yes |
| Paid conversion rate  | 3.19% | 7.04% | [+1.56 pp, +6.15 pp] | Z = 3.264 | 0.0011 | Yes |
| Avg revenue per user | $51.97 | $54.25 | [−$37.60, +$42.16] | t = −0.113 | 0.9100 | No |
| Avg revenue per converted user | $1,115 | $960 | [−$1,827.83, +$108.45] | t = 2.60 | 0.0106 | Yes*|
| Refund rate  | 0.00% | 0.42% | [−0.055 pp, +0.900 pp] | Z = −1.71 | 0.0874 | No |
| Support ticket rate  | 22.0% | 37.3% | [+5.87 pp, +14.14 pp] | Z = 5.00 | < 0.001 | Yes |
| Avg engagement score | 57.0 | 62.9 | [+4.45, +7.37] | t = −7.88 | < 0.001 | Yes |
| Avg days to convert | 8.9 days | 6.4 days | [−4.25, −0.68] | t = 3.11 | 0.0036 | Yes |

Revenue per converted user: statistically significant but CI crosses zero — interpret with caution given n = 22 (Control) and n = 50 (Treatment) converters only.



## 5. Hypothesis Test Interpretation

A two-tailed, two-proportion Z-test was conducted on the primary metric.

| Parameter | Value |
|---|---|
| H₀ | p_treatment − p_control = 0 |
| H₁ | p_treatment − p_control ≠ 0 |
| Test type | Two-proportion Z-test (two-tailed) |
| Significance level | α = 0.05 |
| Z-statistic | 3.264 |
| P-value | 0.0011 |
| 95% Confidence interval | [+1.56 pp, +6.15 pp] |

Decision: p = 0.0011 < 0.05 → Reject H₀

The Treatment group's paid conversion rate of 7.04% is statistically significantly higher than the Control's 3.19%. The 95% CI of [+1.56 pp, +6.15 pp] means the entire plausible range of the true effect is positive — even at the conservative lower bound, the lift remains meaningful. The result is robust and very unlikely to be due to chance.

What the CI adds beyond the p-value: The p-value confirms the effect is real. The CI quantifies how large it is. Even in the worst-case scenario captured by the CI, the Treatment still delivers a +1.56 pp lift — enough to justify the investment in the new onboarding experience.



## 6. Guardrail Analysis

### Refund Rate
- Control: 0.00% | Treatment: 0.42% | Z = −1.71, p = 0.087
- 95% CI: [−0.055 pp, +0.900 pp]
- Assessment: Low risk. Not statistically significant. Only 3 refund events in Treatment. The CI's upper bound of +0.900 pp is small in absolute terms. Monitor post-launch as the converted cohort grows.

### Support Ticket Rate
- Control: 22.0% | Treatment: 37.3% | Z = 5.00, p < 0.001
- 95% CI: [+5.87 pp, +14.14 pp]
- Assessment: HIGH RISK. Statistically significant and practically large. The CI confirms the true increase is at minimum +5.87 pp — a near-certain operational burden at full scale. The product team must identify and fix the friction points driving these tickets before full rollout.

### Revenue per Converted User
- Control: $1,115 | Treatment: $960 | t = 2.60, p = 0.011
- 95% CI: [−$1,827.83, +$108.45]
- Assessment: Watch closely. Technically significant, but the CI is extremely wide due to only 72 converters across both groups and high revenue variance (one user had $8,610 in revenue). The direction is negative — Treatment converts more users but at lower average revenue — consistent with pulling in a broader, less premium-skewed user base. Needs a larger sample before drawing firm conclusions.

### Engagement Score
- Control: 57.0 | Treatment: 62.9 | t = −7.88, p < 0.001
- 95% CI: [+4.45, +7.37 points]
- Assessment: Positive signal. Highly significant with a tight CI. Higher engagement in Treatment suggests users are genuinely more active in the product, not just clicking through the onboarding — this bodes well for long-term retention.



## 7. Segment-Level Insights

**By Region:**
- All four regions show higher conversion in Treatment — the effect is broad-based
- North (+5.4 pp) and South (+4.4 pp) show the strongest lifts
- West shows the smallest lift (+1.7 pp) — worth investigating whether messaging or UX resonates differently

**By Device Type:**
- Mobile users saw the largest conversion lift (7.4% vs 2.6%) — the new experience may be particularly well-suited to mobile
- Desktop also improved meaningfully (6.6% vs 4.5%)

**By Plan Type:**
- Free plan users showed the most dramatic lift (9.3% vs 3.1%) — the campaign is most effective at converting free-tier users
- Premium plan users also improved (6.25% vs 2.75%)
- Basic plan users showed minimal lift (3.9% vs 3.6%) — this segment warrants a dedicated variant

**Key insight:** No subgroup shows a decline in conversion under Treatment. The effect is consistent and directionally positive across all segments, increasing confidence that the result reflects a genuine improvement rather than a segment-specific artefact.



## 8. Final Recommendation

**Decision: Launch with phased rollout**

The evidence for launching is strong:
- Paid conversion rate more than doubled with a tight, fully-positive 95% CI ([+1.56 pp, +6.15 pp])
- Five out of eleven metrics improved significantly; none of the core funnel metrics declined
- The positive effect is consistent across all regions, device types, and plan types

The evidence for caution is also clear:
- Support ticket rate increased by at minimum +5.87 pp (lower bound of CI) — statistically certain and operationally significant
- Revenue per converted user trended downward, though the estimate is imprecise
- Days to convert shortened significantly — worth confirming this reflects genuine engagement rather than pressure-driven impulse conversion

Recommended action plan: Launch Treatment to 25–50% of new users as a phased rollout and conduct qualitative research (session recordings, ticket tagging) to identify which onboarding steps generate the most support contacts and then try to resolve the top friction points; re-measure support ticket rate on the phased cohort. If support ticket rate narrows to within 5 pp of Control, proceed to full launch. To sum it up, we can say that we launch it to a selected segment and continue testing on them.


## 9. Risks and Limitations

- Support ticket spike is certain: full launch without a fix would significantly increase support load
- Revenue per converted user is uncertain: need more converters before drawing conclusions on revenue quality
- Small converted user base: Only 72 converters total limits the precision of all post-conversion metrics
- 30-day observation window: May not reflect true lifetime value; faster conversion (−2.5 days, p = 0.004) could indicate genuine engagement or impulse decisions with higher future churn
- West region underperformance: Smallest lift across all regions — may reflect a geographic messaging or UX mismatch



## 10. Next Steps

1. **Phased launch** to 25–50% of new users immediately
2. **Support investigation** — tag and categorise support tickets from Treatment cohort within 2 weeks
3. **Onboarding friction fix** — sprint to address top ticket-generating steps before full rollout
4. **60-day cohort analysis** — track churn and LTV of Treatment converters vs Control converters to validate revenue quality
5. **Basic plan variant** — design and A/B test an enhanced onboarding variant specifically for Basic plan users
6. **West region deep-dive** — investigate whether creative, messaging, or UX differences explain the lower lift in the West region
