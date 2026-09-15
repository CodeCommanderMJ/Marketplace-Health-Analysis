# Marketplace Health Analysis: Buyer Retention & Seller Churn
**Prepared by:** Misthi Jaiswal
**Data source:** Olist Brazilian E-Commerce Public Dataset (~99,000 orders)
**Purpose:** Identify the biggest risks to marketplace health and recommend concrete actions

---

## Executive Summary

Olist's marketplace shows two distinct, serious health problems: **buyer retention is near-zero across the entire platform**, and **39% of sellers go inactive**, with early revenue and shipping cost as the strongest predictors of which sellers will leave. Neither problem is explained by product quality or pricing alone — both point to structural gaps in how the platform re-engages buyers and supports new sellers.

---

## Finding 1: Buyer Retention Is Critically Low

Across every monthly cohort from 2016–2018, **fewer than 1% of customers place a second order** in any given month after their first purchase. This holds true regardless of acquisition month, meaning it is not a seasonal or one-time issue — it is systemic.

**Why:** Category mix partly explains this — top-selling categories (bed/bath, furniture, home decor) are naturally low-repeat-purchase items. However, even accounting for this, filtering to statistically reliable categories (100+ customers) shows appliances (8.7% repeat rate) and fashion accessories (5.8%) retain customers roughly **2x better** than home decor or bed/bath (~4.5%), despite bed/bath being the single largest acquisition channel.

**Ruled out:** Shipping cost is not a driver — freight-to-price ratio is nearly identical between one-time and repeat buyers (30.8% vs. 32.6%), so this is not a pricing-lever problem.

**Recommendation:** Launch category-based cross-sell campaigns targeting customers acquired through home-decor/bed-bath purchases, nudging them toward higher-repeat categories (appliances, fashion) via post-purchase email or in-app recommendations. Track: 30-day repeat purchase rate among decor-first buyers, before/after campaign.

---

## Finding 2: Seller Churn Is Substantial and Predictable — But Not for the Reason Expected

**39.4% of sellers** have gone inactive (no sale in 90+ days). **18% of sellers made only a single sale, ever.**

**Original hypothesis rejected:** Poor reviews do not drive seller churn — average review scores are nearly identical between active (4.19) and churned (4.05) sellers.

**Actual drivers:** Churned sellers show dramatically lower early performance — 10.6 vs. 47.4 average orders, and ₹1,694 vs. ₹6,275 average revenue compared to active sellers. A Random Forest model built on each seller's **first 90 days only** (avoiding data leakage) identified **early revenue (26% importance)** and **early freight cost (24% importance)** as the two strongest churn predictors — together accounting for half the model's decision-making.

**Recommendation:** Build an early-warning system flagging sellers with low revenue and high freight cost in their first 90 days, and pair this with two interventions:
1. Freight cost subsidies or better logistics support for new sellers in high-shipping-cost regions
2. A "first sale acceleration" program (featured placement, discovery boost) for sellers who haven't hit a revenue threshold by day 30

**Caveat:** The model's recall (20%) is modest — transaction data alone captures only part of the churn story. Recommend supplementing with qualitative seller exit surveys to capture non-data-visible reasons (competing platforms, business closure, etc.).

---

## Priority Recommendation

If only one initiative can be funded this quarter: **the seller early-warning + first-sale acceleration program.** Unlike buyer retention (a broad, platform-wide behavioral pattern that's harder to move quickly), seller churn has a clear, data-backed, and actionable lever — early revenue and freight cost — that the company can influence directly within a seller's first 90 days.

---

## Methodology Note
Analysis performed in Python (Pandas, Scikit-learn) and SQL on the full Olist dataset. Retention measured via cohort analysis (first-purchase month as cohort). Churn model used only pre-90-day features to avoid data leakage from lifetime totals. Full analysis and dashboard available on request.
