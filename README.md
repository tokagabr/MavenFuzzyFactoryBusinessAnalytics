MavenFuzzyFactory — Business Analytics Hackathon

E-Commerce Growth Challenge · Top 3 Finish
Zesto Team · March 2026


Table of Contents

Project Overview
Team
Dataset
Tools Used
Project Structure
Challenges & Analysis
Key Findings
Data Quality
How to Run
Deliverables


Project Overview
MavenFuzzyFactory is a US-based direct-to-consumer e-commerce company selling stuffed animal toys online. It launched in March 2012 with a single product and grew to a four-product portfolio by early 2014.
The challenge: act as an external Business Analytics Consultant. The CEO is heading into a high-stakes investor meeting and needs a data-driven story proving the business has grown efficiently, built a resilient revenue base, and still has significant upside ahead.
We were handed the company's raw data warehouse — unclean, unvalidated — and had to answer 9 business challenges across 4 phases, culminating in a 10-minute investor pitch.

Team
NameRoleZeina AhmedConversion Funnel Analysis (C05)Toka GabrEfficiency & Resilience (C03, C04)Omar El YemaniRepeat Customer Behaviour (C09)Saleh HossamData Quality, Growth Story, Product Performance, Cross-Sell, Capstone

Dataset
TableRowsDescriptionwebsite_sessions472,871One row per site visit — traffic source, device, timingwebsite_pageviews1,188,124One row per page viewed — reveals the conversion funnelorders32,313Completed purchases — ties revenue back to sessionsorder_items40,025Individual line items per order — product and cross-sell analysisorder_item_refunds1,731Refunded items — net revenue and refund rate calculationsproducts4Master product catalogue
Date range: March 19, 2012 — March 19, 2015
Product Catalogue
IDProductLaunchPrice1The Original Mr. FuzzyMarch 2012$49.992The Forever Love BearJanuary 2013$59.993The Birthday Sugar PandaDecember 2013$49.994The Hudson River Mini BearFebruary 2014$24.99

Tools Used

MySQL — all data extraction, cleaning, and analysis
Excel — dashboard with charts for all 6 analysis areas
Canva — final investor presentation deck (20 slides)
Power BI — additional visualizations


Project Structure
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

Challenges & Analysis
Phase 1 — Data Quality & Preparation
Challenge 01 — Is Our Data Fit for Purpose?
Before answering a single business question, we audited the entire dataset.
Issues found:
IssueCount% of DatasetActionDuplicate sessions10.000%Excluded — identical user_id + timestampBad price orders80.001%Excluded — price_usd = 0 or negativeImpossible pageviews100.001%Excluded — pageview timestamp before sessionOrphan product items150.001%Excluded — product_id = 99, not in products tableTotal hard exclusions340.002%Dataset is clean — every metric is defensibleCase/spacing issues89+n/aFixed via LOWER(TRIM()) — not excluded
3-Step Data Governance Recommendation:

At ingestion — reject any record where price_usd ≤ 0 or primary key already exists
At load — run automated duplicate detection on user_id + timestamp combinations
At analysis — run a standard validation script before every sprint checking row counts, null rates, and price ranges

Key fix applied to every query:
sql-- price_usd > 0 goes in JOIN ON, never in WHERE
-- Putting it in WHERE silently converts LEFT JOIN to INNER JOIN
-- and drops all non-converting sessions from session counts

LEFT JOIN orders o
    ON ws.website_session_id = o.website_session_id
    AND o.price_usd > 0   -- correct placement

Phase 2 — The Growth Story
Challenge 02 — How Big Have We Become?
Sessions grew 40x (1,879 → 76,373) and orders grew 100x (59 → 5,908) between Q1 2012 and Q4 2014.
3 inflection points:

Q4 2012 — Billing page redesign lifted conversion rate dramatically
Q1 2013 — Love Bear launch increased AOV and order volume simultaneously
Q4 2014 — Full 4-product portfolio + holiday season = biggest quarter ever

Orders outpaced sessions across every period — meaning the business converted traffic more efficiently as it scaled, not less.
Challenge 03 — Are We Getting Smarter With Every Sale?
MetricQ1 2012Q1 2015ChangeConversion Rate3.14%8.44%+169%Revenue Per Session$1.57$5.30+237%Average Order Value$49.99$62.80+26%
Every improvement traces to a specific, evidence-backed decision — not luck.
Challenge 04 — Have We Built a Resilient Revenue Base?

Channel Resilience Score: 29.2% — built from 0% in 3 years with zero SEO investment
Free + brand orders grew from 0% to 33.5% of total orders
Channels at launch: 1 → Channels by end: 4
If paid advertising stopped tomorrow, 29.2% of revenue would remain completely unaffected


Phase 3 — Customer Experience & Conversion
Challenge 05 — Where Are We Losing Our Customers?
Full conversion funnel (gsearch nonbrand, Aug 2012 onwards):
StepSessionsDrop-off/lander-134,770—/products16,46252.7%Product detail12,07926.6%/cart5,24256.6% ← BIGGEST LEAK/shipping3,58831.6%/billing2,90219.1%/thank-you1,54246.9%
Revenue opportunity: +10pp fix at product detail → cart = +$2,446/month
Challenge 06 — What Was the Return on Our Website Investments?
Landing Page A/B Test (Jun–Jul 2012):

/lander-1 beat /home: bounce rate −5.1pp, conversion +0.88pp
Cumulative incremental revenue since test ended: ~$15,900

Billing Page A/B Test (Sep–Nov 2012):

/billing-2 beat /billing: conversion +17.18pp, rev/session +$8.59
Ongoing monthly value: ~$27,800/month


Phase 4 — Product Strategy & Revenue Mix
Challenge 07 — Which Products Are Pulling Their Weight?
Margin by product (fixed, built into price/cost structure):
ProductMargin %Lifetime RevenueLifetime OrdersBirthday Panda68.5% ← highest$229,0304,980Mini Bear68.4%$150,4895,018Love Bear62.5%$347,2825,789Mr. Fuzzy61.0%$1,210,60724,217
Blended margin expanded from 61.0% → 63.5% over 3 years as higher-margin products entered the mix.
Strategic tension flagged: Birthday Panda has the highest gross margin (68.5%) but a persistent 5–7% refund rate with no improvement over 15 months. Net effective margin drops to ~64.4%.
Zero cannibalization confirmed: Mr. Fuzzy grew from 59 to 1,557 primary orders/month with no sustained drop at any product launch.
Supplier change impact (Mr. Fuzzy):
PeriodRefund RateBefore Jun–Aug 20148.00%Transition Sep 201413.26% ← arm defect peakAfter Oct–Dec 20143.28% ← better than pre-defect baseline
Challenge 08 — Are We Leaving Money in the Cart?
Cross-sell feature launched September 25, 2013.
Attachment rate matrix (after Dec 5, 2014):
PrimaryMini BearBirthday PandaLove BearMr. Fuzzy20.89% · 933 orders12.38% · 553 orders5.33% · 238 ordersLove Bear20.38% · 260 orderslow—Birthday Panda22.41% ← highestlowlowMini Bear—3.79%2.40%
Recommendation: Feature Mini Bear at cart for every primary product. Mr. Fuzzy + Mini Bear generated $74,621 in 3.5 months (~$21,800/month).
Challenge 09 — How Valuable Are Our Repeat Customers?
MetricValueRepeat rate13.4%Avg days to return32.5 days (min 1, max 69)Paid nonbrand repeat sessionsExactly 0Free channel repeat share66.8%Repeat conv rate vs new8.11% vs 7.25% (+0.86pp)LTV multiple2.67x ($12.42 vs $4.65)
Returning customers never come back through paid nonbrand ads — they return through organic, direct, and brand channels at zero acquisition cost.

Capstone — The Strategic Growth Roadmap
Challenge 10 — Why We Will Win
$100,000 Budget Allocation:
InvestmentBudgetExpected Monthly ReturnProduct detail A/B test$40,000~$26,700/monthMini Bear cart optimisation$25,000~$2,374/monthPaid nonbrand scale-up$35,000Est. 3–7× ROASTotal$100,000$29,074+/month confirmed
Top 2 Risks:

Birthday Panda persistent 6.04% refund rate — apply new supplier framework immediately
55–57% paid channel concentration — integrate ad spend data and invest in organic

Missing data point that would most change recommendations: Ad spend and CPC data per channel — without it, paid media ROI is estimated from revenue per session, not true ROAS.

Key Findings
#FindingNumber1Sessions growth40x (1,879 → 76,373)2Orders growth100x (59 → 5,908)3Conversion rate improvement3.14% → 8.44% (+169%)4Revenue per session improvement$1.57 → $5.30 (+237%)5Channel Resilience Score29.2%6Free + brand order share33.5%7Biggest funnel leakProduct detail → cart 43.4%8Billing page monthly value~$27,800/month ongoing9Best margin productBirthday Panda 68.5%10Best quality productMini Bear max 2.41% refund11Mini Bear attachment rate20–22% across all primaries12LTV multiple2.67x returning vs single visit13Paid nonbrand repeat sessionsExactly 014Total data exclusions34 records / 0.002%

Data Quality
All queries in this project apply the following fixes consistently:
sql-- Fix 1: Text field standardization
LOWER(TRIM(utm_source))
LOWER(TRIM(utm_campaign))
LOWER(TRIM(device_type))
LOWER(pageview_url)

-- Fix 2: Bad price exclusion (in JOIN ON, never in WHERE)
AND o.price_usd > 0

-- Fix 3: Orphan product exclusion
WHERE oi.product_id != 99

-- Fix 4: Organic vs direct detection
COALESCE(TRIM(http_referer), '')

-- Fix 5: Socialbook exclusion
WHERE (LOWER(TRIM(utm_source)) != 'socialbook' OR utm_source IS NULL)

-- Fix 6: LEFT JOIN integrity
-- (product_id != 99 OR product_id IS NULL) on LEFT JOINs
-- to keep non-converting sessions in session counts

How to Run

Import the MavenFuzzyFactory schema into MySQL
Run queries in order: C01 → C02 → C03/04 → C05 → C06 → C07 → C08 → C09 → C10
Export results as CSV from MySQL Workbench
Open MavenFuzzyFactory_Dashboard_Final.xlsx and refresh data

sqlUSE mavenfuzzyfactory;

-- Start with data quality check
SOURCE sql/challenge_01_data_quality.sql;

Deliverables
FileDescriptionchallenge_01_data_quality.sqlFull data audit — 6 issue categories, 34 exclusionschallenge_02_the_growth_story.sqlQuarterly sessions, orders, channel breakdownchallenge_03_04_efficiency_resilience.sqlConv rate, AOV, rev/session, channel resilience scorechallenge_05_conversion_funnel.sqlFull funnel, mobile vs desktop, revenue opportunitychallenge_06_website_AB_test_roi.sqlLanding page + billing page A/B test ROIchallenge_07_product_performance.sqlRevenue, margin, refunds, supplier change impactchallenge_08_cross_sell_analysis.sqlPre/post cross-sell feature, attachment rate matrixchallenge_09_repeat_customer_behaviour.sqlRetention, LTV, channel mix new vs repeatchallenge_10_capstone_strategic_roadmap.sqlFull KPI summary, $100K budget allocationMavenFuzzyFactory_Dashboard_Final.xlsxExcel dashboard with charts for all 6 analysis areas

Results
Top 3 finish at the MavenFuzzyFactory Business Analytics Hackathon
Presented to judges as a 10-minute investor pitch with 5-minute live Q&A
Every number in the deck traces back to a confirmed, audited SQL result

Zesto Team · March 2026
