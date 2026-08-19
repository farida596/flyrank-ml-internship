# Content Opportunity & Refresh Scoring Engine

## 1. Abstract
* **Problem Statement:** Organic search traffic decays over time if published content is not actively audited and updated. Manual auditing is inefficient for large-scale publishers.
* **Methodology:** We developed an automated, leakage-free Content Opportunity Scoring framework combining search impressions, click-through rates (CTR), and current SERP positioning.
* **Key Findings:** The scoring system successfully surfaces underperforming, high-potential assets that raw impression metrics overlook, establishing a clear operational queue for editorial teams.

---

## 2. Introduction & Problem Context
Maintaining high organic visibility requires continuous content refresh workflows. Content editors often struggle to prioritize which articles to update first due to conflicting metrics like high impressions vs. low rankings. 

This research establishes an objective, data-driven framework that translates search performance analytics into actionable editorial queues, ensuring engineering and content resources are allocated to high-ROI update opportunities.

---

## 3. Data & Feature Engineering
* **Data Ingestion:** Extracted Google Search Console analytics data containing impressions, clicks, click-through rate (CTR), and search position.
* **Data Cleaning & Filtering:** Filtered out non-actionable queries, handled zero-division cases in CTR calculations, and structured historical performance windows.
* **Leakage Prevention Strategy:** Implemented explicit runtime assertions (`assert 'clicks_last_30d' not in df_scored.columns`) to guarantee that future metric data never leaks into the scoring engine calculations.

---

## 4. Methodology & Priority Scoring Logic
To balance potential search demand against current execution gaps, we implemented the following priority scoring formula:

$$\text{Priority Score} = \frac{\text{Impressions} \times (1 - \text{CTR})}{\text{Position} + 1}$$

* **Formula Rationale:**
  * $\text{Impressions} \times (1 - \text{CTR})$ highlights content with massive search volume that fails to convert into clicks.
  * Dividing by $(\text{Position} + 1)$ penalizes pages ranking far beyond the first page, prioritizing pages on the verge of top-tier SERP visibility.

---

## 5. Results & Baseline Comparison
* **Baseline Approach:** Standard sorting by raw impressions often prioritizes already successful top-ranking pages, leading to redundant updates with minimal upside.
* **Scoring Engine Approach:** Our Opportunity Score surfaces "hidden gem" pages—high-volume topics stuck on positions 6–15 with underperforming CTRs.
* **Impact:** The top 10 recommended updates identified by the engine represent a higher overall traffic lift potential compared to the raw impression baseline.

---

## 6. Action Playbook for Content Editors
Each score is paired with automated reason codes to direct editorial action:

* **High Volume, Low CTR (Position 1–5):** Optimize title tags, meta descriptions, and search intent alignment to improve clickability.
* **Striking Distance (Position 6–15):** Update outdated statistics, expand key sections, and strengthen internal linking to push onto Page 1.
* **Low Traffic & Poor Ranking (Position 15+):** Evaluate for consolidation, redirecting, or full structural rewrites.

---

## 7. Limitations & Future Improvements
* **Limitations:** The engine currently assumes linear ranking difficulty and does not account for seasonality fluctuations or search intent shifts.
* **Future Work:** Incorporate semantic embeddings to analyze content freshness directly alongside LLM-driven competitor gap analysis.

---

## 8. Embedded Visualizations

### Priority Score Distribution Summary
The distribution of priority scores highlights a small tail of ultra-high-opportunity targets alongside a broad baseline of routine content:

| Priority Score Range | Segment Classification | Recommended Primary Action |
| :--- | :--- | :--- |
| **85.0 +** | Critical Priority | Immediate CTR & Title Tag Optimization |
| **50.0 – 84.9** | High Priority | Content Expansion & Internal Link Building |
| **20.0 – 49.9** | Medium Priority | Routine Data Refresh & Fact Checks |
| **< 20.0** | Low Priority | Maintain / Monitor Baseline |

---

## 9. Conclusion & References
This capstone project provides an end-to-end, reproducible framework for automated content auditing. By bridging search analytics with algorithmic prioritization, content teams can systematically maximize organic search efficiency.

### References
1. Google Search Console API Documentation
2. Search Engine Optimization Performance Benchmarks & Ranking Distribution Studies
