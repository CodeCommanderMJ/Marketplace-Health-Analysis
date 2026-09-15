<div align="center">

# 🛒 Marketplace Health Analysis
### Buyer Retention & Seller Churn on a Real E-Commerce Marketplace

*A Product Analytics case study on the Olist Brazilian E-Commerce dataset — a real multi-sided marketplace, structurally similar to Meesho, Flipkart, and Amazon (buyers + sellers + logistics, not single-sided retail).*

</div>

---

## 🎯 TL;DR

> **Fewer than 1% of buyers ever return to purchase again — and it's not about shipping cost.**
> **39% of sellers go inactive — and it's not about bad reviews either.**
> Both problems have a real, fixable driver hiding underneath the obvious guess. This project finds it, tests it, and recommends what to do about it.

---

## 📌 Why This Project

Most portfolio projects stop at "here's a chart." This one is built the way a Product Analyst actually works at a marketplace company: **form a hypothesis → test it against data → reject it if it's wrong → find the real driver → make an evidence-backed recommendation.** Two of the three hypotheses below turned out to be *wrong* — and figuring that out is the actual point of the project.

---

## ❓ Business Questions & Answers

| # | Question | Answer |
|---|---|---|
| 1 | Do buyers come back and purchase again? Which segments retain best? | **No — under 1% do, in any cohort.** Appliances & fashion accessories retain ~2x better than home decor/bed-bath. |
| 2 | Do sellers stay active, or quit after their first sale — and why? | **39.4% churn; 18% sell only once.** Driven by low early revenue and high early freight cost — not review quality. |
| 3 | Does shipping/freight cost affect buyer loyalty or seller sustainability? | **Not for buyers** (freight-to-price ratio is nearly identical for repeat vs. one-time buyers). **Yes for sellers** — it's a top-2 churn predictor. |
| 4 | Can we flag at-risk sellers *before* they churn, using only early signals? | **Partially.** A Random Forest on first-90-day data reaches 43% precision / 20% recall — useful but not production-grade; early revenue & freight cost are the clearest signals. |

---

## 🔍 Key Findings

### 1️⃣ Buyer retention is critically low — platform-wide, every cohort, no exceptions
Across every monthly cohort from 2016–2018, **under 1% of customers** place a second order in any given month after their first purchase.

- ❌ **Hypothesis rejected:** shipping cost is *not* the cause — one-time and repeat buyers show nearly identical freight-to-price ratios (**30.8% vs. 32.6%**).
- ✅ **Real driver — category:** filtering to statistically reliable sample sizes (100+ customers), **appliances (8.7%)** and **fashion accessories (5.8%)** retain customers **~2x better** than home decor or bed/bath (**~4.5%**) — despite bed/bath being the single largest acquisition channel.

### 2️⃣ Seller churn is substantial (39.4%) — and the obvious guess was wrong
**18% of sellers made only one sale, ever.**

- ❌ **Hypothesis rejected:** review score is *not* a meaningful churn driver — active sellers average **4.19⭐**, churned sellers **4.05⭐**. Practically no difference.
- ✅ **Real drivers:** a Random Forest model trained *only* on each seller's first 90 days (to prevent data leakage from lifetime totals) found **early revenue** and **early freight cost** are the top two predictors — together explaining **~50%** of the model's decisions.

### 3️⃣ The churn model — honest results, not oversold
| Model | Recall (churned sellers) | Precision |
|---|---|---|
| Logistic Regression (baseline) | 0% ⚠️ | 0% |
| Random Forest (`class_weight='balanced'`) | 20% | 43% |

The baseline model completely failed on the imbalanced label — a textbook case of why **accuracy alone is misleading** (70% "accuracy" while catching zero churners). Random Forest improved this meaningfully, and even with modest recall, its **feature importance** produced a real, actionable insight.

---

## 💡 Recommendations

| Finding | Recommendation | Success Metric |
|---|---|---|
| Retention is category-driven | Cross-sell campaigns nudging decor/bed-bath buyers toward appliances & fashion | 30-day repeat rate, by first-category cohort |
| Churn tied to early revenue & freight cost | Early-warning flag + "first-sale acceleration" support for new sellers in high-freight regions | 90-day seller survival rate |

📄 Full reasoning, caveats, and priority call: **[business_memo.md](./business_memo.md)**

---

## 🛠️ Tech Stack

- **Python** — Pandas, NumPy (cleaning, cohort analysis, feature engineering)
- **SQL** (SQLite) — exploratory querying on the merged transactional dataset
- **Scikit-learn** — Logistic Regression & Random Forest for churn prediction
- **Matplotlib / Seaborn** — exploratory visualization
- **Power BI** — final interactive dashboard

---

## 🧠 Methodology Notes
*(the details I'd expect an interviewer to probe — answered upfront)*

- **Payment table deduplication** — orders paid via multiple methods (e.g., voucher + credit card) created duplicate rows on a naive merge, inflating order/revenue counts. Fixed by aggregating payments per `order_id` *before* merging.
- **Cohort definition** — used `customer_unique_id`, not `customer_id` (unique per *order*, not per person, in this dataset) — a common Olist-specific gotcha that silently breaks retention analysis if missed.
- **Data leakage avoidance** — the churn model uses only each seller's first-90-day activity, never lifetime totals, since lifetime order count is circularly related to churn status by definition.
- **Sample-size discipline** — category-level repeat-rate claims are only reported for categories with 100+ customers, after early results showed misleadingly high rates from categories with under 20 customers.

---

## 📁 Repo Structure

```
├── notebooks/
│   └── olist_analysis.ipynb        # Full analysis: cleaning → retention → churn → ML
├── dashboard/
│   ├── olist_marketplace_dashboard.pbix
│   └── dashboard_screenshot.png
├── business_memo.md                # One-page findings & recommendations
└── README.md
```

> Raw data (~100MB) is not included in this repo — see **Data Source** below.

---

## 🗂️ Data Source

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — ~99,000 real orders (2016–2018), via Kaggle.

---

<div align="center">

## 👤 Author

**Misthi Jaiswal**
[LinkedIn](#) · [GitHub](#) · [Email](mailto:misthijaiswal0012@gmail.com)

</div>
