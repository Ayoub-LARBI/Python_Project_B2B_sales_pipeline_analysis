# 📊 B2B Sales Pipeline & Revenue Analysis

**Business question:** Why are sales opportunities being won or lost, and where might the company be leaving revenue on the table? 💡

**Dataset:** [Maven Analytics CRM Sales Opportunities](https://mavenanalytics.io/) — ~8,800 B2B hardware sales opportunities across four tables (`sales_pipeline`, `accounts`, `products`, `sales_teams`).

**Tools:** Python (Pandas, NumPy, Matplotlib, Seaborn), Jupyter.

---

## 🌱 Business problem

The sales team has 8,800 recorded opportunities but no clear read on *why* deals are won or lost, which products and sectors deserve more focus, or how much of the current open pipeline is realistically still winnable. This project turns the raw CRM export into five evidence-based findings and concrete recommendations for sales management.

## ❓ Analytical questions

1. How does the pipeline perform overall?
2. What characteristics are associated with Won vs. Lost opportunities?
3. Does sales-cycle length relate to win rate?
4. Where is potential revenue concentrated or being lost?
5. What actionable findings can be given to sales management?

## 🛠️ Methodology

- **Win rate** = `Won / (Won + Lost)` — open opportunities excluded, since their outcome isn't known yet.
- **Sales cycle** = `close_date - engage_date`, defined only for closed opportunities.
- **Grain:** one row = one sales opportunity.
- Every finding below was stress-tested for an alternative explanation before being accepted — see Sections 9–12 of the notebook for the full reasoning trail (cross-validation, not just description). 🔍

## 🧹 Data quality

- Missing `close_date`/`close_value` is structural (open deals haven't closed yet), not an error.
- `Prospecting` deals have no `engage_date` — opportunity age cannot be computed for them; stated as a limitation rather than estimated.
- Found and fixed a silent product-name mismatch (`'GTXPro'` vs. `'GTX Pro'`) between the pipeline and products tables before it could break any merge. 🐛➡️✅

## 📈 Key findings

| # | Finding | Evidence | Validation |
|---|---|---|---|
| 1 | Short sales cycles (≤14 days) have a lower win rate | 55.5% vs. 68.7% (13.2pp gap), threshold-tested across 7 cutoffs | Holds across products and sectors |
| 2 | 3 of 7 products generate 83.5% of Won revenue | Revenue decomposed into volume × deal size; win rates cluster 60–65% | Confirmed via decomposition, not assumption |
| 3 | Overall agent rankings are confounded by product mix | Agent win rate recomputed within one product (GTX Basic) | Ranking scrambles when product is held fixed |
| 4 | 81% of open "Engaging" deals have exceeded the longest-ever winning cycle | $949,737 in estimated exposure, concentrated in one product | Confirmed on a large, reliable sample (excluded a small-sample false positive) |
| 5 | Sector appears to have little effect on win rate overall | Win rate within one product (GTX Pro) swings 54.5% → 71.9% by sector | Confirmed once product mix is held fixed |

### 🖼️ Visuals

**Won revenue by product**

![Won revenue by product](visuals\1_Won_revenue_by_product.png)

**Win rate by sales-cycle threshold**

![Win rate by threshold](visuals\2_Win_rate_by_sales_cycle_threshold.png)

**Agent win rate: overall vs. GTX Basic only**

![Agent ranking scramble](visuals\3_Agent_win_rate_overall_vs_GTX_Basic_only.png)

**Stalled pipeline exposure by product**

![Stalled exposure by product](visuals\4_Stalled_Pipeline_Exposure_by_Product.png)

**GTX Pro win rate by sector**

![GTX Pro win rate by sector](visuals\5_GTX Pro_win_rate_by_sector.png)

## ✅ Recommendations

Each recommendation is tied to a specific finding, with its limitation stated alongside it — no action goes beyond what the data supports.

1. **Sales-cycle signal:** Prioritize follow-up calls on deals past day 14; use them to surface what's driving continued buyer interest.
2. **Revenue concentration:** When agents must choose where to spend limited time among similarly-likely deals, prioritize the top three revenue-generating products — same conversion odds, far higher payoff per win.
3. **Agent performance:** Before reassigning agents by "best product," recompute per-agent, per-product win rates on adequate sample sizes — a raw leaderboard is not a fair comparison.
4. **Stalled pipeline:** Trigger a proactive check-in call for deals past the historical max winning cycle; the call — not the day count alone — decides whether to re-engage or close.
5. **Sector effect:** Once sample sizes are confirmed adequate, prioritize sales effort in higher-performing sectors for the affected product line.

## ⚠️ Limitations

- Revenue ≠ profit — no cost data exists, so findings describe "highest-revenue," never "most profitable."
- Sales-cycle findings describe *completed* cycles only; they cannot predict the outcome of a currently-open short-cycle deal.
- Exposure estimates use list price as a stand-in for close value — not a revenue forecast.
- Several sub-group comparisons (sector × product, agent × product) rely on modest sample sizes; each finding above states where that applies.

## 📂 Repo structure

```
├── data/                 # sales_pipeline.csv, accounts.csv, products.csv
├── notebooks/            # full analysis notebook
├── visuals/              # exported chart images (referenced above)
├── README.md
└── requirements.txt
```

## 🧰 Tools used

Python · Pandas · NumPy · Matplotlib · Seaborn · Jupyter · Git/GitHub
