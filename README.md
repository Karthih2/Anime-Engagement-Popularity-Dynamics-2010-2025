<img src="https://img.shields.io/badge/Anime%20Analytics-MyAnimeList%202010--2025-FF6B35?style=for-the-badge&logo=databricks" width="100%">

<h1 align="center">Anime Engagement & Popularity Dynamics</h1>

<p align="center">
  <b>Medallion Architecture • Feature Engineering • Statistical Analysis • BI Dashboards</b><br>
  <i>Data Engineering • EDA • Correlation Analysis • Time Series • Power BI • Tableau</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?style=flat-square&logo=python">
  <img src="https://img.shields.io/badge/PySpark-3.x-orange?style=flat-square&logo=apache-spark">
  <img src="https://img.shields.io/badge/Databricks-Community-red?style=flat-square&logo=databricks">
  <img src="https://img.shields.io/badge/PowerBI-Dashboard-yellow?style=flat-square&logo=powerbi">
  <img src="https://img.shields.io/badge/Tableau-Public-lightblue?style=flat-square&logo=tableau">
  <img src="https://img.shields.io/badge/Dataset-MyAnimeList-green?style=flat-square">
</p>

---

## ⛩️ Executive Summary

> *"A full end-to-end data analytics pipeline investigating whether anime quality drives popularity — built on Databricks Medallion Architecture with statistical validation, EDA, time series analysis and BI dashboards across Power BI and Tableau."*

This project analyses **3,818 anime titles** from MyAnimeList (2010–2025) to answer a core industry question:

**Does more anime production lead to better content — and does quality actually drive popularity?**

The central finding is a confirmed **Anime Quality Paradox**:
- More production ≠ better quality
- More popularity ≠ higher quality
- Quality moderately drives popularity **(Pearson r = 0.567, p < 0.001)**
- The industry self-corrected post-2021 — Modern Renaissance era produces the highest quality scores in the dataset

---

## 📂 Repository Structure
```
📁 notebooks/
   ├── 01_data_ingestion.ipynb            → Bronze layer — raw CSV ingestion
   ├── 02_data_cleaning.ipynb             → Silver layer — cleaning & filtering
   ├── 03_gold_feature_engineering.ipynb  → Gold layer — feature engineering
   ├── 04_EDA_Analysis.ipynb              → 18 research questions across EDA
   ├── 05_Correlation_Analysis.ipynb      → Pearson & Spearman correlation
   └── 06_Time_Series.ipynb               → YoY production & quality trends

📁 dashboards/
   ├── PowerBI_Dashboard.pbix             → 3-page interactive business dashboard
   └── Tableau_Story.twbx                 → 5-slide narrative story

📁 data/
   └── gold_analysis.csv                     → Exported gold layer dataset

📁 report/
   └── Anime_Engagement_Popularity_Dynamics.pdf

📁 Visualization/
   ├── EDA       
   ├── Correlation Analysis          
   ├── Time Series Analysis

```
---

## 🌐 Background & Overview

The global anime industry has undergone dramatic transformation over the past 15 years.

- Streaming platforms like **Crunchyroll** and **Netflix** accelerated internationalisation
- Annual production **doubled** from ~140 titles in 2010 to a peak of ~280 by 2016–2017
- By 2025 the global anime market reached approximately **USD 36–38 billion**, growing at **7–9% CAGR**

This rapid expansion created a fundamental tension between **production volume** and **content quality** — and between critical quality and audience popularity. This project quantifies that tension using a full Medallion Architecture data pipeline on Databricks, covering ingestion, cleaning, feature engineering, statistical analysis and business intelligence visualisation.

**Project Type:** End-to-end data analytics portfolio project
**Domain:** Media & Entertainment Analytics
**Period:** 2010 – 2025
**Platform:** Databricks Community Edition

---

## 📊 Dataset Overview

| Property | Value |
|---|---|
| Source | MyAnimeList (MAL) |
| Raw dataset | 28,858 titles (full MAL catalogue) |
| After year filter (2010–2026) | 3,818 titles |
| Scored anime | ~3,018 (79%) |
| Unscored anime | ~800 (21%) |
| Score range | 2.5 – 9.3 |
| Members range | 34 – 4,192,911 |
| Unique genres | 21 |
| Unique studios | 500+ |

### Missing Value Treatment

| Column | Missing % | Treatment | Reason |
|---|---|---|---|
| demographics | 56.87% | "Unknown" | Too high to impute reliably |
| title_english | 30.35% | Fill with title | Logical fallback |
| themes | 28.33% | "None" | Categorical — not numeric |
| score | 21.25% | 0 | Unrated ≠ average score |
| scored_by | 21.25% | 0 | Consistent with score = 0 |
| studios | 12.83% | "Unknown" | Categorical |
| genres | 6.01% | "None" | Categorical |
| episodes | 4.04% | Median (12) | Standard seasonal length |
| rating | 1.92% | "Unknown" | Categorical |
| rank | 2.44% | 0 | Numeric placeholder |

> **Design Decision:** Score filled with 0 rather than mean/median.
> Imputing average would artificially inflate quality metrics for unrated anime.
> All score-based analysis filters score > 0 before calculation.

---

## 🔧 Technical Process

🏗️ Pipeline — Medallion Architecture
 
```
Raw CSV  ──►  🥉 Bronze Layer  ──►  🥈 Silver Layer  ──►  🥇 Gold Layer  ──►  📊 Analysis & BI
              Ingestion              Cleaning &              Feature              EDA · Correlation
                                     Filtering               Engineering          Time Series · Dashboards
```
<img src="Anime Engagement Analytics Architecture Diagram.png" width="650"/>

### 🔧 Feature Engineering — Gold Layer
 
| Feature | Formula | Purpose |
|:---|:---|:---|
| `engagement_score` | `log(members) + log(favorites+1)` | Composite audience interest; log normalises right-skew |
| `score_z` | `(score − mean) / std` | Quality normalised vs full dataset; `score > 0` only |
| `popularity_momentum` | `members / (scored_by+1)` | High = viral casual · Low = dedicated fanbase |
| `retention_proxy` | `favorites / (members+1)` | Proportion of viewers who became loyal fans |
| `score_tier` | Score range buckets | Excellent ≥8.0 · Good ≥7.0 · Average ≥6.0 · Below Average >0 |
| `airing_era` | Year-based labels | Digital Boom · Streaming Revolution · Global Expansion · Modern Renaissance |

> **Assumptions & Caveats:**
> - Log transformation applied throughout to normalise skewed distributions
> - Genre analysis restricted to genres with 50+ titles (small sample bias prevention)
> - Studio quality analysis restricted to studios with 10+ anime (same reason)
> - Popularity momentum set to 0 for unscored anime
> - Score z-score returns null for unscored anime (not extreme negative value)

### 📚 Analysis Notebooks
 
| Notebook | Method | Output |
|:---|:---|:---|
| `04_EDA_Analysis` | 18 research questions — distributions, genres, studios, correlations | Charts, tier breakdown, hidden gems, overhyped titles |
| `05_Correlation_Analysis` | Pearson (score vs log members) + Spearman (score vs raw members); genre-level for 20+ observations | r = 0.567, genre correlation table |
| `06_Time_Series` | YoY production growth, 3-year rolling average, quality trend, genre growth 2010–2017 vs 2018–2025 | Production cycle, quality recovery curve |
 
---

## 📈 Key Results
### 🎌 Core Statistical Finding
 
| Metric | Value |
|:---|:---|
| Pearson r (Score vs Log Members) | **0.567** |
| Spearman r (Score vs Members) | **0.568** |
| p-value | **< 0.001** |
| Interpretation | Moderate positive — quality influences but does not solely determine popularity |
 
### 🏆 Quality at a Glance
 
| Metric | Value |
|:---|:---|
| Total anime analysed | 3,818 |
| Excellent tier (score ≥ 8.0) | **8% only** |
| Best quality year | **2024** — avg score 7.06 |
| Worst quality year | **2017** — avg score 6.61 |
| Quality swing (2017 → 2024) | **+0.45 points** |
| Top genre by score (50+ titles) | **Drama — 7.28** |
| Top studio by score (10+ titles) | **Ufotable — 7.98** |
| Audience drop-off | 677M members → 347M rated → 7M favorites |
 
### 🌸 Era Quality Analysis
 
| Era | Period | Avg Score Z | Verdict |
|:---|:---:|:---:|:---:|
| Digital Boom Era | 2010–2012 | +0.140 | ✅ Above average |
| Streaming Revolution | 2013–2016 | −0.115 | ❌ Below average |
| Global Expansion Era | 2017–2020 | −0.133 | ❌ Worst era |
| **Modern Renaissance** | **2021–2025** | **+0.143** | 🏆 **Best era** |
 
### 🎭 Genre-Level Correlation
 
| Genre | Pearson r | Relationship |
|:---|:---:|:---|
| Sports | 0.633 | Quality strongly predicts popularity |
| Supernatural | 0.628 | Quality strongly predicts popularity |
| Sci-Fi | 0.603 | Quality strongly predicts popularity |
| Drama | 0.556 | Moderate relationship |
| Action | 0.551 | Moderate relationship |
| Romance | 0.441 | Weak — audience preference dominates |
 
### 🎛️ Dashboards
 
**Power BI — 3-Page Interactive Dashboard**
 
| Page | Title | Content |
|:---:|:---|:---|
| 1 | Project Overview | 5 key findings · 5 analytical dimensions |
| 2 | Overview | KPIs · top genres · score tier donut · year / season / source slicers |
| 3 | Analysis | Production trend · era diverging bar · engagement funnel · scatter plot |
 
**Tableau — 5-Slide Narrative Story** &nbsp;·&nbsp; *"From Boom to Renaissance: The Evolution of Anime Quality"*
 
| Slide | Title | Chart Type |
|:---:|:---|:---|
| 1 | Did Growth Kill Quality? | Dual-axis bar + line |
| 2 | The Streaming Effect | Diverging bar by era |
| 3 | Quality Has a Genre | Horizontal bar with colour gradient |
| 4 | Popular ≠ Good | Scatter with r = 0.57 trend line annotation |
| 5 | Hidden Gems vs Broken Promises | Scatter with category colours |
 
---

## 🔍 Insights & Deep Dive

### ✔ Insight 1 — Excellence is Rare but Recovering

Only **8%** of 3,818 anime achieve Excellent status (8.0+). The largest group is
Average at 29.4%, Good at 28.3%, Unscored at 21% and Below Average at 13.2%.
Despite rarity, the Modern Renaissance era (2021–2025) shows avg z-score +0.143 —
the highest of all four eras. The worst year was 2017 at 6.61.
By 2024 average recovered to 7.06. Less output, better results.

### ✔ Insight 2 — Volume Dilutes Genre Quality

Comedy has 1,478 titles and scores ~6.85. Drama has 411 titles and scores 7.28.
The pattern is consistent across all genres: more titles = lower average score.
Suspense and Drama also build the most loyal fanbases by retention proxy.
The same genres that score highest retain fans most deeply.
Action viewers watch casually. Drama viewers become fans.

### ✔ Insight 3 — Franchise Loyalty Overrides Quality

Yakusoku no Neverland S2 dropped from **8.7 → 5.25** (−3.45 points, largest in dataset)
yet retained **973,450 members**. Tokyo Ghoul:re scores 6.37 but holds 1.26M members.
Two distinct overhype drivers: sequel disappointments and fanservice titles.
Franchise equity sustains audiences long after quality has deteriorated.

### ✔ Insight 4 — Streaming First Hurt Then Helped Quality

Production peaked at ~280 titles in 2016–2017. Average score hit lowest at 6.61 in 2017.
The inverse relationship is statistically visible. Post-2021 platforms shifted to
prestige content investment. The Modern Renaissance recovery to +0.143 confirms
the correction is real and measurable in the data.

### ✔ Insight 5 — Best Anime Rarely Reach Mainstream

Hidden gems (score_z > 1, scored_by ≥ 1,000, members ≥ 5,000, popularity > 5,000):

| Title | Score | Members | Score Z |
|---|---|---|---|
| IDOLiSH7 Third Beat! Part 2 | 8.34 | 17,101 | +1.68 |
| Chiikawa | 8.26 | 10,352 | +1.59 |
| Pui Pui Molcar | 8.01 | 18,750 | +1.30 |
| Love Live! Superstar!! 3rd | 7.89 | 20,223 | +1.16 |

All four are niche continuations with passionate small fanbases.
Outstanding quality exists throughout the catalogue but visibility is
determined by format, franchise and genre reach — not by score alone.

---

## 🏁 Recommendations
 
### 🎬 For Studios
- **Prioritise quality over volume** — selective production consistently outperforms high-volume output on score, retention and brand value
- **Invest in Drama, Suspense and Mystery** — highest average scores and strongest fan loyalty in the dataset
- **Protect sequel quality** — the Neverland collapse (−3.45 pts) is the clearest data-backed example of franchise equity destruction
### 📺 For Streaming Platforms
- **Volume-based acquisition does not improve catalogue quality** — the 2013–2020 period is direct evidence
- **Surface hidden gems** through recommendation systems — `score_z > 1`, `popularity rank > 5,000` titles represent an underserved audience with strong loyalty signals
- **Prestige content investment works** — it produced the measurable quality recovery seen in the Modern Renaissance
### 📊 For Industry Analysts
- **Track annual production volume as a leading quality indicator** — the inverse cycle is observable and predictable
- **Score trajectory across sequential seasons** is more reliable than raw member retention for franchise health assessment
---

## ⚠️ Limitations
 
| # | Limitation |
|:---:|:---|
| 1 | Data sourced from MyAnimeList only — may skew toward active online communities over casual global viewers |
| 2 | Multi-valued genre entries require Python-side exploding — not natively supported in all BI tools |
| 3 | Ratings reflect only users who submitted scores — passive viewers may hold different quality opinions |
| 4 | 2024–2025 data is partially incomplete — recently aired titles had not accumulated sufficient ratings at collection time |
| 5 | Member and favorites counts are cumulative and do not reflect when engagement occurred relative to air date |
 
---

## 🚀 Tech Stack
 
![Python](https://img.shields.io/badge/Python-Pandas%20·%20NumPy%20·%20SciPy-3776AB?style=flat-square&logo=python&logoColor=white)
&nbsp;
![PySpark](https://img.shields.io/badge/PySpark-Databricks%20Community-E25A1C?style=flat-square&logo=apache-spark&logoColor=white)
&nbsp;
![Power BI](https://img.shields.io/badge/Power%20BI-Interactive%20Dashboard-F2C811?style=flat-square&logo=powerbi&logoColor=black)
&nbsp;
![Tableau](https://img.shields.io/badge/Tableau-Narrative%20Story-1F7FBF?style=flat-square&logo=tableau&logoColor=white)
&nbsp;
![Stats](https://img.shields.io/badge/Statistics-Pearson%20·%20Spearman%20·%20Time%20Series-4CAF50?style=flat-square)
 
---

## 🙌 Author

**Karthick S**  

---

## 🏷️ Tags
 
`data-analytics` &nbsp; `anime` &nbsp; `databricks` &nbsp; `pyspark` &nbsp; `medallion-architecture` &nbsp; `feature-engineering` &nbsp; `eda` &nbsp; `correlation-analysis` &nbsp; `time-series` &nbsp; `power-bi` &nbsp; `tableau` &nbsp; `media-analytics` &nbsp; `portfolio-project`
 
---
<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=140&section=footer&animation=twinkling" width="100%"/>

<br/>

✨ <i>Built with data, curiosity, and an unhealthy amount of anime knowledge.</i> ✨  
🎌 <b>Exploring the dynamics of anime popularity — one dataset at a time.</b>

<br/><br/>

</div>
