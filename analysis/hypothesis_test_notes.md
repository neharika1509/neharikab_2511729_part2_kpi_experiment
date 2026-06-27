# Hypothesis Test Notes

## Primary Metric Being Tested
**Paid Conversion Rate** — proportion of users who converted to a paid subscription within 30 days.

This metric was selected because:
- It is the most direct measure of the campaign's stated objective (improve user conversion)
- It has clear business value: each converted user generates immediate subscription revenue
- It is a clean binary outcome per user, making it well-suited for a proportion test
- It links directly to the launch decision: if the new experience doesn't move conversions, there is no financial justification to ship it

---

## Hypothesis Framing

### Null Hypothesis (H₀)
There is no difference in paid conversion rate between the Control and Treatment groups.

> H₀: p_treatment − p_control = 0

### Alternate Hypothesis (H₁)
The Treatment group has a different paid conversion rate than the Control group.

> H₁: p_treatment − p_control ≠ 0

---

## Test Design

| Parameter | Value |
|---|---|
| Test type | Two-proportion Z-test |
| Tails | Two-tailed |
| Significance level (α) | 0.05 |
| Confidence interval | 95% |

**Why two-tailed?** A two-tailed test is used to detect both improvement and harm. This is appropriate for a business launch decision — we want to know if the new experience hurts conversion just as much as if it helps.

**Why two-proportion Z-test?** Both the Control and Treatment metrics are binary proportions (converted / not converted) with large enough sample sizes (n > 500 per group) for the normal approximation to hold.

---

## Test Inputs & Results

| | Control | Treatment |
|---|---|---|
| Users (n) | 690 | 710 |
| Conversions | 22 | 50 |
| Conversion Rate | 3.19% | 7.04% |

**Pooled proportion (p̂):** (22 + 50) / (690 + 710) = **5.14%**

**Standard Error:**
$$SE = \sqrt{\hat{p}(1-\hat{p})\left(\frac{1}{n_1}+\frac{1}{n_2}\right)} = \sqrt{0.0514 \times 0.9486 \times \left(\frac{1}{690}+\frac{1}{710}\right)} = 0.00837$$

**Test Statistic:**
$$Z = \frac{p_{trt} - p_{ctrl}}{SE} = \frac{0.0704 - 0.0319}{0.00837} = 3.264$$

**P-value:** 2 × (1 − Φ(3.264)) = **0.0011**

**95% Confidence Interval on the difference** (using unpooled SE):
$$CI = (p_{trt} - p_{ctrl}) \pm 1.96 \times SE_{unpooled} = +3.85\text{ pp} \pm 2.29\text{ pp}$$
$$\Rightarrow \textbf{[+1.56 pp, +6.15 pp]}$$

---

## Full Hypothesis Test Output Table

All 11 metrics were tested and results are fully documented in the **Overall Summary** sheet of `experiment_summary.xlsx`. Below is a summary of the statistical outputs for each metric:

| Metric | Test Used | Test Statistic | p-value | 95% CI | Significant? |
|---|---|---|---|---|---|
| Landing page visit rate | Z-test | Z = 3.54 | 0.0004 | [+3.90 pp, +13.64 pp] | **Yes** |
| Trial start rate | Z-test | Z = 1.66 | 0.0970 | [−0.71 pp, +8.59 pp] | No |
| Onboarding completion rate | Z-test | Z = 2.64 | 0.0083 | [+1.43 pp, +9.52 pp] | **Yes** |
| **Paid conversion rate ⭐** | **Z-test** | **Z = 3.264** | **0.0011** | **[+1.56 pp, +6.15 pp]** | **Yes** |
| Avg revenue per user | t-test | t = −0.113 | 0.9100 | [−$37.60, +$42.16] | No |
| Avg revenue per converted user | t-test | t = 2.60 | 0.0106 | [−$1827.83, +$108.45] | Yes* |
| Refund rate 🔴 | Z-test | Z = −1.71 | 0.0874 | [−0.055 pp, +0.900 pp] | No |
| Support ticket rate 🔴 | Z-test | Z = 5.00 | < 0.001 | [+5.87 pp, +14.14 pp] | **Yes** |
| Avg engagement score | t-test | t = −7.88 | < 0.001 | [+4.45, +7.37] | **Yes** |
| Avg days to convert | t-test | t = 3.11 | 0.0036 | [−4.25, −0.68] | **Yes** |

*Revenue per converted user: CI crosses zero — interpret cautiously given small n (22 vs 50 converters).

---

## Interpretation Logic

**Decision rule:** Reject H₀ if p-value < 0.05.

**Primary metric result:** p = 0.0011 < 0.05 → **Reject H₀**

The Treatment group's paid conversion rate (7.04%) is statistically significantly higher than the Control group's (3.19%). The 95% CI of [+1.56 pp, +6.15 pp] tells us the true lift is likely between 1.56 and 6.15 percentage points — the entire interval is above zero, confirming the direction of effect with confidence.

**Business interpretation:** The new onboarding and activation campaign more than doubled paid conversion rate. This is a large, statistically reliable effect. The evidence strongly supports that the Treatment experience causally improves user conversion.

**Key guardrail finding:** Support ticket rate increased significantly (Z = 5.00, p < 0.001, CI: [+5.87 pp, +14.14 pp]). This is a material operational risk and must be addressed before full rollout.

**What the CI tells us that the p-value alone does not:**
- The p-value tells us the effect is real (not due to chance)
- The CI tells us the *magnitude* of the effect and how uncertain we are about it
- A [+1.56 pp, +6.15 pp] CI means even in the most conservative scenario, the Treatment still delivers a meaningful lift — the lower bound alone would justify the launch investment
