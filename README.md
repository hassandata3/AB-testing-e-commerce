# A/B Test: E-commerce Checkout Button

Did a redesigned checkout button make more people buy? Analysed in Python and SQL, ending in a ship / don't ship recommendation.

**Result: ship it.** Conversion rose from **9.50% to 13.53%** — a lift of **+4.03 percentage points (+42.4%)**, with revenue per user up **$2.77**.

---

## The experiment

| | |
|---|---|
| Design | 6,000 users, 50/50 split, 14 days |
| Control | old checkout button |
| Treatment | new checkout button |
| Primary metric | conversion rate |
| Secondary | revenue per user |
| Guardrail | average order value |

## Results

| Metric | Control | Treatment | Change |
|---|---|---|---|
| Conversion | 9.50% | 13.53% | **+4.03 pp** |
| Revenue / user | $6.65 | $9.42 | **+$2.77** |
| Avg order value | $70.03 | $69.59 | flat |

**Evidence:** z = 4.90, p < 0.001, 95% CI **[+2.42, +5.65]** points. The interval clears zero, so the effect is unlikely to be chance.

**Power:** MDE at 80% power was +2.23 pp; the observed lift was +4.03 pp. The test was sized to find an effect this large, so the result isn't a marginally-powered fluke.

## What the numbers mean

Average order value didn't move. The entire revenue gain came from **more people buying, not bigger baskets** — so if the goal is larger orders, this change won't get you there; test upsells instead.

Desktop shows a bigger lift than mobile (+6.02 vs +2.70 pp), but mobile's observed effect sits *below* its own MDE, meaning that segment is underpowered and its estimate would move around on a re-run. Treated as a hypothesis for the next test, not a finding.

## Method

1. Deduplicate — 12 repeated rows removed (6,012 → 6,000)
2. Check the split — chi-square, 3,052 vs 2,948, passes
3. Conversion rate and lift, absolute and relative
4. Two-proportion z-test (pooled SE)
5. Confidence interval (unpooled SE — different question, different formula)
6. Power / minimum detectable effect
7. Revenue via bootstrap, since 88% of users spend $0 and a t-test doesn't fit that shape
8. Segment by device, with per-segment MDE
9. Decision

## Files

| File | What it is |
|---|---|
| `checkout_ab.ipynb` | full analysis (Colab-ready) |
| `analysis_mysql.sql` | same workflow in SQL, through to the confidence interval |
| `EXCEL_GUIDE.md` | pivot-table and formula walkthrough |
| `checkout_ab_test.csv` | dataset |
| `ab_test_carousel.pdf` | 5-slide summary |

**Tools:** pandas · numpy · scipy · statsmodels · matplotlib · MySQL
