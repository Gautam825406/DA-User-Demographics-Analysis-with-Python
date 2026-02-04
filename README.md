# Demographics Analysis with Python (Website Audience by City)

## Problem
Businesses need to understand **who their website visitors are** and **how different audience segments behave** so they can:
- Target marketing more efficiently
- Improve engagement and retention
- Allocate ad budget to the highest-value segments

This project performs a **demographics analysis using location (Town/City)** and engagement metrics to identify:
- Which cities drive the most users
- Which cities have the strongest engagement quality
- Which segments are underperforming / high potential
- A data-driven **budget allocation strategy** for marketing

---

## Dataset
The analysis uses a user-demographics dataset with the following key columns:

- `Town/City`, `Country`
- `Users`, `New users`
- `Engaged sessions`, `Engagement rate`
- `Engaged sessions per user`, `Average engagement time`
- `Event count`

> Note: This dataset is location-focused (no age/gender). The demographics analysis here is primarily **geographic + behavioral**.

---

## Approach

### 1) Data Validation & Cleaning
- Validated required columns and numeric types
- Checked for nulls and invalid ranges (e.g., engagement rate outside 0–1)
- Standardized text fields (`Town/City`, `Country`)

### 2) Feature Engineering
Created additional metrics to make the analysis more actionable:
- `Returning users = Users - New users`
- `New_user_share = New users / Users`
- `Returning_user_share = Returning users / Users`
- `Events_per_user = Event count / Users`
- `Events_per_engaged_session = Event count / Engaged sessions`

### 3) Exploratory Analysis (EDA)
Generated interactive visualizations for:
- Top cities by Users
- Engagement rate by city
- Engaged sessions per user
- Average engagement time
- New vs Returning users by city
- Event count by city

### 4) Segmentation (Resume-level)
Segmented cities across three dimensions:

**A) User Scale Segment (Quantiles)**
- Low Users / Medium Users / High Users

**B) Engagement Segment (Composite Engagement Score)**
Built `Engagement_Score` using standardized (z-scored) metrics:
- Engagement rate
- Engaged sessions per user
- Avg engagement time
- Events per user

Segmented into:
- Low Engagement / Medium Engagement / High Engagement

**C) Growth Segment (New User Share Quantiles)**
- Low Growth (More Returning)
- Medium Growth
- High Growth (More New)

Combined into an actionable label:
`User_Segment | Engagement_Segment | Growth_Segment`

### 5) Budget Allocation Strategy
Created a **Priority Score** to recommend budget share by city:

- Reach proxy: `log(1 + Users)`
- Engagement quality: `Engagement_Score`
- Growth proxy: `New_user_share`

Then normalized and weighted:
- Reach: 0.45
- Engagement: 0.40
- Growth: 0.15

Finally:
- `Budget_Share = Priority_Score / sum(Priority_Score)`

This produces a ranked list of cities where marketing budget is expected to yield better returns.

---

## Key Insights (What to Look For)
Your actual findings depend on the dataset values, but typical outcomes include:

### 1) User Concentration
A few major cities usually contribute a disproportionate share of users (common in India datasets), indicating:
- Strong brand presence in metros
- Opportunity to expand into Tier-2 cities with targeted campaigns

### 2) Engagement Quality ≠ User Volume
Top user cities are not always the most engaged.
- Some smaller cities may have **high engagement time and high session depth**
- These can be “high-intent” markets worth scaling

### 3) New vs Returning Mix (Retention Signal)
If most cities show very high new-user share and low returning share:
- It signals a retention opportunity (onboarding, personalization, lifecycle campaigns)

### 4) Recommended Budget Allocation
Budget should not be based on Users alone.
The Priority Score emphasizes:
- Cities with strong reach + strong engagement
- High-growth markets with decent engagement that can be nurtured

---

## Outputs
The notebook exports:

- `outputs/demographics_with_segments.csv`  
  City-level dataset with engineered features + segments + budget share

- `outputs/segment_summary.csv`  
  Segment-level summary table (counts, total users, avg engagement, avg budget share)

---

## Next Steps (Improvements for a Production-Ready Project)

### 1) Add Marketing Funnel Data (True Conversion Rate)
To compute real conversion rate, add:
- Sessions / clicks / leads / signups / purchases

Then evaluate:
- City-wise conversion rate
- CAC by city (if spend is available)

### 2) Add Time Dimension (Trends)
If data includes daily/weekly values:
- Identify rising or declining cities
- Seasonality by city
- Engagement changes after campaigns or releases

### 3) Statistical Testing
Validate whether engagement differences are significant:
- Compare engagement rates between city groups using hypothesis testing
- Identify outliers using robust methods

### 4) Clustering (Unsupervised Segmentation)
Use K-Means / HDBSCAN on normalized engagement + growth + reach features to discover natural city clusters instead of quantile rules.

### 5) Personalization Strategy per Segment
Convert segments into marketing actions:
- High Users + High Engagement → retention, upsell, loyalty
- High Users + Low Engagement → UX/content optimization
- Low Users + High Engagement → awareness + scaling campaigns
- High Growth + Medium Engagement → onboarding and conversion flows

---

## Tech Stack
- Python
- Pandas, NumPy
- Plotly (interactive charts)
- SciPy (stats utilities)

---

## How to Run
1. Place the dataset file in the same folder as the notebook:
   - `user-demographics.csv`

2. Run notebook cells top-to-bottom.

3. Exported files will be saved under:
   - `outputs/`
