# MavenFuzzyFactory — Business Analytics Hackathon

> **E-Commerce Growth Challenge · Top 3 Finish**
> Zesto Team · March 2026

---

## Table of Contents

- [Project Overview](#project-overview)
- [Team](#team)
- [Dataset](#dataset)
- [Tools Used](#tools-used)
- [Project Structure](#project-structure)
- [Challenges & Analysis](#challenges--analysis)
- [Key Findings](#key-findings)
- [Data Quality](#data-quality)
- [How to Run](#how-to-run)
- [Deliverables](#deliverables)

---

## Project Overview

MavenFuzzyFactory is a US-based direct-to-consumer e-commerce company selling stuffed animal toys online. It launched in March 2012 with a single product and grew to a four-product portfolio by early 2014.

The challenge: act as an external Business Analytics Consultant. The CEO is heading into a high-stakes investor meeting and needs a data-driven story proving the business has grown efficiently, built a resilient revenue base, and still has significant upside ahead.

We were handed the company's raw data warehouse — unclean, unvalidated — and had to answer 9 business challenges across 4 phases, culminating in a 10-minute investor pitch.

---

## Team

| Name | Role |
|---|---|
| Zeina Ahmed | Conversion Funnel Analysis (C05) |
| Toka Gabr | Efficiency & Resilience (C03, C04) |
| Omar El Yemani | Repeat Customer Behaviour (C09) |
| Saleh Hossam | Data Quality, Growth Story, Product Performance, Cross-Sell, Capstone |

---

## Dataset

| Table | Rows | Description |
|---|---|---|
| website_sessions | 472,871 | One row per site visit — traffic source, device, timing |
| website_pageviews | 1,188,124 | One row per page viewed — reveals the conversion funnel |
| orders | 32,313 | Completed purchases — ties revenue back to sessions |
| order_items | 40,025 | Individual line items per order — product and cross-sell analysis |
| order_item_refunds | 1,731 | Refunded items — net revenue and refund rate calculations |
| products | 4 | Master product catalogue |

**Date range:** March 19, 2012 — March 19, 2015

### Product Catalogue

| ID | Product | Launch | Price |
|---|---|---|---|
| 1 | The Original Mr. Fuzzy | March 2012 | $49.99 |
| 2 | The Forever Love Bear | January 2013 | $59.99 |
| 3 | The Birthday Sugar Panda | December 2013 | $49.99 |
| 4 | The Hudson River Mini Bear | February 2014 | $24.99 |

---

## Tools Used

- **MySQL** — all data extraction, cleaning, and analysis
- **Excel** — dashboard with charts for all 6 analysis areas
- **Canva** — final investor presentation deck (20 slides)
- **Power BI** — additional visualizations

---

## Project Structure

```
mavenfuzzyfactory/
│
├── sql/
│   ├── challenge_01_data_quality.sql
│   ├── challenge_02_the_growth_story.sql
│   ├── challenge_03_04_efficiency_resilience.sql
│   ├── challenge_05_conversion_funnel.sql
│   ├── challenge_06_website_AB_test_roi.sql
│   ├── challenge_07_product_performance.sql
│   ├── challenge_08_cross_sell_analysis.sql
│   ├── challenge_09_repeat_customer_behaviour.sql
│   └── challenge_10_capstone_strategic_roadmap.sql
│
├── dashboard/
│   └── MavenFuzzyFactory_Dashboard_Final.xlsx
│
├── presentation/
│   └── MavenFuzzyFactory_Deck.pptx
│
└── README.md
```

---

## Challenges & Analysis

### Phase 1 — Data Quality & Preparation

#### Challenge 01 — Is Our Data Fit for Purpose?

| Issue | Count | % of Dataset | Action |
|---|---|---|---|
| Duplicate sessions | 1 | 0.000% | Excluded — identical user_id + timestamp |
| Bad price orders | 8 | 0.001% | Excluded — price_usd = 0 or negative |
| Impossible pageviews | 10 | 0.001% | Excluded — pageview timestamp before session |
| Orphan product items | 15 | 0.001% | Excluded — product_id = 99 |
| **Total hard exclusions** | **34** | **0.002%** | Dataset is clean |
| Case/spacing issues | 89+ | n/a | Fixed via LOWER(TRIM()) |

**3-Step Data Governance Recommendation:**
1. At ingestion — reject any record where price_usd ≤ 0 or primary key already exists
2. At load — run automated duplicate detection on user_id + timestamp combinations
3. At analysis — run a standard validation script before every sprint

**Key fix applied to every query:**
```sql
LEFT JOIN orders o
    ON ws.website_session_id = o.website_session_id
    AND o.price_usd > 0   -- in ON not WHERE
```

---

### Phase 2 — The Growth Story

#### Challenge 02 — How Big Have We Become?

Sessions grew **40x** (1,879 → 76,373) and orders grew **100x** (59 → 5,908).

**3 inflection points:**
- **Q4 2012** — Billing page redesign
- **Q1 2013** — Love Bear launch
- **Q4 2014** — Full 4-product portfolio + holiday season

#### Challenge 03 — Are We Getting Smarter With Every Sale?

| Metric | Q1 2012 | Q1 2015 | Change |
|---|---|---|---|
| Conversion Rate | 3.14% | 8.44% | +169% |
| Revenue Per Session | $1.57 | $5.30 | +237% |
| Average Order Value | $49.99 | $62.80 | +26% |

#### Challenge 04 — Have We Built a Resilient Revenue Base?

- **Channel Resilience Score: 29.2%** — built from 0% in 3 years with zero SEO investment
- Free + brand orders: 0% → **33.5%** of total
- Channels: 1 → 4

---

### Phase 3 — Customer Experience & Conversion

#### Challenge 05 — Where Are We Losing Our Customers?

| Step | Sessions | Drop-off |
|---|---|---|
| /lander-1 | 34,770 | — |
| /products | 16,462 | 52.7% |
| Product detail | 12,079 | 26.6% |
| **/cart** | **5,242** | **56.6% ← BIGGEST LEAK** |
| /shipping | 3,588 | 31.6% |
| /billing | 2,902 | 19.1% |
| /thank-you | 1,542 | 46.9% |

Revenue opportunity: +10pp fix = **+$2,446/month**

#### Challenge 06 — What Was the Return on Our Website Investments?

| Test | Lift | Monthly Value |
|---|---|---|
| Landing Page A/B (Jun–Jul 2012) | +0.88pp conversion | ~$497/month · $15,900 cumulative |
| Billing Page A/B (Sep–Nov 2012) | +17.18pp conversion | **~$27,800/month ongoing** |

---

### Phase 4 — Product Strategy & Revenue Mix

#### Challenge 07 — Which Products Are Pulling Their Weight?

| Product | Margin % | Lifetime Revenue | Lifetime Orders |
|---|---|---|---|
| Birthday Panda | **68.5%** ← highest | $229,030 | 4,980 |
| Mini Bear | 68.4% | $150,489 | 5,018 |
| Love Bear | 62.5% | $347,282 | 5,789 |
| Mr. Fuzzy | 61.0% | $1,210,607 | 24,217 |

Blended margin: **61.0% → 63.5%** over 3 years.

**Supplier change impact (Mr. Fuzzy):**

| Period | Refund Rate |
|---|---|
| Before Jun–Aug 2014 | 8.00% |
| Transition Sep 2014 | 13.26% ← arm defect |
| After Oct–Dec 2014 | **3.28% ← better than ever** |

#### Challenge 08 — Are We Leaving Money in the Cart?

| Primary | Mini Bear | Birthday Panda | Love Bear |
|---|---|---|---|
| Mr. Fuzzy | **20.89%** · 933 orders | 12.38% · 553 orders | 5.33% · 238 orders |
| Love Bear | **20.38%** · 260 orders | low | — |
| Birthday Panda | **22.41% ← highest** | low | low |
| Mini Bear | — | 3.79% | 2.40% |

Mr. Fuzzy + Mini Bear generated **$74,621 in 3.5 months** (~$21,800/month).

#### Challenge 09 — How Valuable Are Our Repeat Customers?

| Metric | Value |
|---|---|
| Repeat rate | 13.4% |
| Avg days to return | 32.5 days |
| Paid nonbrand repeat sessions | **Exactly 0** |
| Free channel repeat share | 66.8% |
| LTV multiple | **2.67x** ($12.42 vs $4.65) |

---

### Capstone — The Strategic Growth Roadmap

#### Challenge 10 — Why We Will Win

**$100,000 Budget Allocation:**

| Investment | Budget | Expected Monthly Return |
|---|---|---|
| Product detail A/B test | $40,000 | ~$26,700/month |
| Mini Bear cart optimisation | $25,000 | ~$2,374/month |
| Paid nonbrand scale-up | $35,000 | Est. 3–7× ROAS |
| **Total** | **$100,000** | **$29,074+/month** |

**Top 2 Risks:**
1. Birthday Panda 6.04% refund rate — apply new supplier framework immediately
2. 55–57% paid channel concentration — integrate ad spend data, invest in organic

---

## Key Findings

| # | Finding | Number |
|---|---|---|
| 1 | Sessions growth | 40x (1,879 → 76,373) |
| 2 | Orders growth | 100x (59 → 5,908) |
| 3 | Conversion rate | 3.14% → 8.44% (+169%) |
| 4 | Revenue per session | $1.57 → $5.30 (+237%) |
| 5 | Channel Resilience Score | 29.2% |
| 6 | Free + brand order share | 33.5% |
| 7 | Biggest funnel leak | Product detail → cart 43.4% |
| 8 | Billing page monthly value | ~$27,800/month |
| 9 | Best margin product | Birthday Panda 68.5% |
| 10 | Best quality product | Mini Bear max 2.41% refund |
| 11 | Mini Bear attachment rate | 20–22% across all primaries |
| 12 | LTV multiple | 2.67x |
| 13 | Paid nonbrand repeat sessions | Exactly 0 |
| 14 | Total data exclusions | 34 records / 0.002% |

---

## Data Quality

All queries apply these fixes consistently:

```sql
-- Text standardization
LOWER(TRIM(utm_source))
LOWER(TRIM(utm_campaign))
LOWER(TRIM(device_type))
LOWER(pageview_url)

-- Bad price exclusion (in JOIN ON, never WHERE)
AND o.price_usd > 0

-- Orphan product exclusion
WHERE oi.product_id != 99

-- Organic vs direct detection
COALESCE(TRIM(http_referer), '')

-- Socialbook exclusion
WHERE (LOWER(TRIM(utm_source)) != 'socialbook' OR utm_source IS NULL)

-- LEFT JOIN integrity
(product_id != 99 OR product_id IS NULL)
```

---

## How to Run

```sql
USE mavenfuzzyfactory;
SOURCE sql/challenge_01_data_quality.sql;
```

1. Import the MavenFuzzyFactory schema into MySQL
2. Run queries in order: C01 → C02 → C03/04 → C05 → C06 → C07 → C08 → C09 → C10
3. Export results as CSV from MySQL Workbench
4. Open `MavenFuzzyFactory_Dashboard_Final.xlsx`

---

## Deliverables

| File | Description |
|---|---|
| `challenge_01_data_quality.sql` | Full data audit — 6 issue categories, 34 exclusions |
| `challenge_02_the_growth_story.sql` | Quarterly sessions, orders, channel breakdown |
| `challenge_03_04_efficiency_resilience.sql` | Conv rate, AOV, rev/session, resilience score |
| `challenge_05_conversion_funnel.sql` | Full funnel, mobile vs desktop, revenue opportunity |
| `challenge_06_website_AB_test_roi.sql` | Landing page + billing page A/B test ROI |
| `challenge_07_product_performance.sql` | Revenue, margin, refunds, supplier change |
| `challenge_08_cross_sell_analysis.sql` | Pre/post cross-sell, attachment rate matrix |
| `challenge_09_repeat_customer_behaviour.sql` | Retention, LTV, channel mix new vs repeat |
| `challenge_10_capstone_strategic_roadmap.sql` | KPI summary, $100K budget allocation |
| `MavenFuzzyFactory_Dashboard_Final.xlsx` | Excel dashboard with charts |

---

## Results

**Top 3 finish** at the MavenFuzzyFactory Business Analytics Hackathon
Presented as a 10-minute investor pitch with 5-minute live Q&A
Every number traces back to a confirmed, audited SQL result

---

*Zesto Team · March 2026*
