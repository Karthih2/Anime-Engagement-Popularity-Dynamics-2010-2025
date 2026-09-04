<img src="https://img.shields.io/badge/Anime%20Analytics-MyAnimeList%202010--2025-FF6B35?style=for-the-badge&logo=databricks" width="100%">

<h1 align="center">🎌 Anime Engagement & Popularity Dynamics</h1>

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

This study presents an end-to-end analytical pipeline examining **3,818 anime titles** (MyAnimeList, 2010–2025) to investigate the following research question:

> **Does increased production volume correspond to higher content quality, and does quality serve as a meaningful predictor of audience popularity?**

**Principal Finding — The Anime Quality Paradox**
- Increased production volume does not correspond to improved quality
- Higher popularity does not correspond to higher quality
- Quality is a moderate, statistically significant predictor of popularity — **Pearson r = 0.567, p < 0.001**
- Industry-wide quality metrics improved following 2021, with the **Modern Renaissance** era recording the highest average quality scores in the dataset

---

## 📂 Repository Structure

<details>
<summary><b>Click to expand folder layout</b></summary>
<br>

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
   └── gold_analysis.csv                  → Exported gold layer dataset

📁 report/
   └── Anime_Engagement_Popularity_Dynamics.pdf

📁 Visualization/
   ├── EDA
   ├── Correlation Analysis
   └── Time Series Analysis
```

</details>

---

## 🌐 Background & Overview

The global anime industry has undergone substantial transformation over the past fifteen years:

- Streaming platforms such as **Crunchyroll** and **Netflix** accelerated the industry's internationalization
- Annual production volume **doubled**, rising from approximately 140 titles in 2010 to a peak of approximately 280 titles during 2016–2017
- By 2025, the global anime market was valued at approximately **USD 36–38 billion**, growing at a compound annual growth rate of 7–9%

This expansion introduced a tension between production volume and content quality, as well as between critical quality and audience popularity. This project quantifies that tension through a full Medallion Architecture pipeline: ingestion → cleaning → feature engineering → statistical analysis → business intelligence.

| | |
|---|---|
| **Project Type** | End-to-end data analytics portfolio project |
| **Domain** | Media & Entertainment Analytics |
| **Period** | 2010 – 2025 |
| **Platform** | Databricks Community Edition |

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

<details>
<summary><b>Click to expand missing value treatment table</b></summary>
<br>

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

> **Design Decision:** Score was imputed with 0 rather than the mean or median, as imputing an average would artificially inflate quality metrics for unrated titles. All score-based analyses filter on `score > 0` prior to calculation.

</details>

---

## 🔧 Technical Process

### 🏗️ Pipeline — Medallion Architecture

```
Raw CSV ──► 🥉 Bronze Layer ──► 🥈 Silver Layer ──► 🥇 Gold Layer ──► 📊 Analysis & BI
            Ingestion            Cleaning &          Feature           EDA · Correlation
                                  Filtering           Engineering       Time Series · Dashboards
```

<img src="Anime Engagement Analytics Architecture Diagram.png" width="650"/>

### Feature Engineering — Gold Layer

| Feature | Formula | Purpose |
|---|---|---|
| `engagement_score` | `log(members) + log(favorites+1)` | Composite audience interest metric; log transformation normalizes right-skew |
| `score_z` | `(score − mean) / std` | Quality normalized relative to the full dataset; computed for `score > 0` only |
| `popularity_momentum` | `members / (scored_by+1)` | High values indicate viral/casual reach; low values indicate a dedicated fanbase |
| `retention_proxy` | `favorites / (members+1)` | Proportion of viewers who became loyal fans |
| `score_tier` | Score range buckets | Excellent ≥8.0 · Good ≥7.0 · Average ≥6.0 · Below Average >0 |
| `airing_era` | Year-based labels | Digital Boom · Streaming Revolution · Global Expansion · Modern Renaissance |

> **Assumptions & Caveats**
> - Log transformation was applied throughout to normalize skewed distributions
> - Genre-level analysis was restricted to genres with 50+ titles to mitigate small-sample bias
> - Studio-level quality analysis was restricted to studios with 10+ titles for the same reason
> - Popularity momentum is set to 0 for unscored titles
> - Score z-score returns null for unscored titles rather than an extreme negative value

### 📚 Analysis Notebooks

| Notebook | Method | Output |
|---|---|---|
| `04_EDA_Analysis` | 18 research questions covering distributions, genres, studios, and correlations | Charts, tier breakdown, hidden gems, overhyped titles |
| `05_Correlation_Analysis` | Pearson (score vs. log members) and Spearman (score vs. raw members) correlation; genre-level analysis for 50+ observations | r = 0.567, genre correlation table |
| `06_Time_Series` | Year-over-year production growth, 3-year rolling average, quality trend, genre growth comparison for 2010–2017 vs. 2018–2025 | Production cycle, quality recovery curve |

---

## 📈 Key Results

### 🎌 Core Statistical Finding

| Metric | Value |
|---|---|
| Pearson r (Score vs. Log Members) | **0.567** |
| Spearman r (Score vs. Members) | **0.568** |
| p-value | **< 0.001** |
| Interpretation | Moderate positive correlation — quality is an influencing factor but not the sole determinant of popularity |

### 🏆 Quality at a Glance

| Metric | Value |
|---|---|
| Total anime analyzed | 3,818 |
| Excellent tier (score ≥ 8.0) | **8%** |
| Best quality year | **2024** — avg score 7.06 |
| Worst quality year | **2017** — avg score 6.61 |
| Quality swing (2017 → 2024) | **+0.45 points** |
| Top genre by score (50+ titles) | **Drama — 7.28** |
| Top studio by score (10+ titles) | **Ufotable — 7.98** |
| Audience drop-off | 677M members → 347M rated → 7M favorites |

### 🌸 Era Quality Analysis

| Era | Period | Avg Score Z | Verdict |
|---|:---:|:---:|:---:|
| Digital Boom Era | 2010–2012 | +0.140 | ✅ Above average |
| Streaming Revolution | 2013–2016 | −0.115 | ❌ Below average |
| Global Expansion Era | 2017–2020 | −0.133 | ❌ Lowest-performing era |
| **Modern Renaissance** | **2021–2025** | **+0.143** | 🏆 **Highest-performing era** |

### 🎭 Genre-Level Correlation

| Genre | Pearson r | Relationship |
|---|:---:|---|
| Sports | 0.633 | Quality strongly predicts popularity |
| Supernatural | 0.628 | Quality strongly predicts popularity |
| Sci-Fi | 0.603 | Quality strongly predicts popularity |
| Drama | 0.556 | Moderate relationship |
| Action | 0.551 | Moderate relationship |
| Romance | 0.441 | Weak relationship — audience preference dominates |

### 🎛️ Dashboards

<details>
<summary><b>Click to expand dashboard breakdown</b></summary>
<br>

**Power BI (3 Pages)**

* **Overview:** Key findings and analytical dimensions
* **Dashboard:** KPIs, top genres, score tiers, slicers
* **Analysis:** Trends, eras, engagement, scatter plot

**Tableau (5-Story Dashboard)**

* Growth vs. Quality
* Streaming Effect
* Genre Quality
* Popular ≠ Good
* Hidden Gems vs. Broken Promises

</details>

---

## 🔍 Insights & Deep Dive

<details>
<summary><b>Insight 1 — Excellence Is Rare but Recovering</b></summary>
<br>

* Only **8%** of titles fall within the **Excellent (8.0+)** tier
* The remaining titles are distributed across **Average (29.4%)**, **Good (28.3%)**, **Unscored (21%)**, and **Below Average (13.2%)**
* The **Modern Renaissance (2021–2025)** recorded the highest average **z-score (+0.143)** among all eras
* Average scores recovered from **6.61 in 2017** to **7.06 in 2024**
* These findings support an association between reduced output and improved quality outcomes

</details>

<details>
<summary><b>Insight 2 — Volume Dilutes Genre Quality</b></summary>
<br>

* **Comedy:** 1,478 titles, average score of **6.85**
* **Drama:** 411 titles, average score of **7.28**
* Genres with higher title counts generally exhibit lower average scores
* **Drama** and **Suspense** demonstrate stronger fan retention than **Action**

</details>

<details>
<summary><b>Insight 3 — Franchise Loyalty Overrides Quality</b></summary>
<br>

* *Yakusoku no Neverland S2:* score declined from **8.70 to 5.25 (−3.45)**, while retaining **973,450 members**
* *Tokyo Ghoul:re:* score of **6.37** with **1.26M members**
* These cases indicate that franchise popularity can sustain audience size despite declining critical quality

</details>

<details>
<summary><b>Insight 4 — Streaming First Hurt, Then Helped Quality</b></summary>
<br>

* Production peaked at approximately **280 titles during 2016–2017**
* Average score reached its lowest recorded point of **6.61 in 2017**
* Quality metrics improved following **2021**
* The **Modern Renaissance (+0.143)** confirms this recovery

</details>

<details>
<summary><b>Insight 5 — Best Anime Rarely Reach Mainstream</b></summary>
<br>

Hidden gems (`score_z > 1`, `scored_by ≥ 1,000`, `members ≥ 5,000`, `popularity > 5,000`):

| Title | Score | Members | Score Z |
|---|---|---|---|
| IDOLiSH7 Third Beat! Part 2 | 8.34 | 17,101 | +1.68 |
| Chiikawa | 8.26 | 10,352 | +1.59 |
| Pui Pui Molcar | 8.01 | 18,750 | +1.30 |
| Love Live! Superstar!! 3rd | 7.89 | 20,223 | +1.16 |

All four titles are niche continuations with small, highly engaged fanbases, indicating that high-quality content exists throughout the catalogue but visibility depends on format, franchise recognition, and genre reach rather than score alone.

</details>

---

## 🏁 Recommendations

<details>
<summary><b>For Studios</b></summary>
<br>

- Prioritize quality over volume — selective production consistently outperforms high-volume output on score, retention, and brand value
- Invest in Drama, Suspense, and Mystery genres, which exhibit the highest average scores and strongest fan loyalty
- Safeguard sequel quality — the Neverland decline (−3.45 points) illustrates measurable franchise equity erosion

</details>

<details>
<summary><b>For Streaming Platforms</b></summary>
<br>

- Volume-based content acquisition does not improve catalogue quality, as demonstrated by the 2013–2020 period
- Surface hidden gems through recommendation systems (`score_z > 1`, `popularity rank > 5,000`) to reach an underserved, loyal audience segment
- Prestige content investment is effective, as evidenced by the measurable quality recovery observed in the Modern Renaissance

</details>

<details>
<summary><b>For Industry Analysts</b></summary>
<br>

- Track annual production volume as a leading (inverse) indicator of quality
- Use score trajectory across sequential seasons, rather than raw member retention alone, to assess franchise health

</details>

---

## ⚠️ Limitations

| # | Limitation |
|:---:|---|
| 1 | Data is sourced exclusively from MyAnimeList, which may skew toward active online communities relative to casual global viewers |
| 2 | Multi-valued genre entries require Python-side exploding, which is not natively supported across all BI tools |
| 3 | Ratings reflect only users who submitted scores; passive viewers may hold differing quality assessments |
| 4 | 2024–2025 data is partially incomplete, as recently aired titles had not accumulated sufficient ratings at the time of collection |
| 5 | Member and favorite counts are cumulative and do not reflect the timing of engagement relative to the air date |

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

`data-analytics` `anime` `databricks` `pyspark` `medallion-architecture` `feature-engineering` `eda` `correlation-analysis` `time-series` `power-bi` `tableau` `media-analytics` `portfolio-project`

---

**📌 Data Coverage Note**

> A small number of **2026 records** are present in the source data. Since **2026 is an incomplete observation period**, its lower production and audience figures reflect **partial data coverage** and should not be compared directly with the complete **2010–2025** years.
<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=140&section=footer&animation=twinkling" width="100%"/>

<br/>

<i>✨ An end-to-end analytical study of production volume, content quality, and audience engagement in the global anime industry, 2010–2025. ✨</i>

</div>
