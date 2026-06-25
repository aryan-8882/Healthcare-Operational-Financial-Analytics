# Healthcare Operational & Financial Analytics Pipeline
### An End-to-End Data Analysis Capstone Project for Hospital Operations Using Python & Jupyter Notebooks

---

## 📌 Project Overview
This repository contains an end-to-end data engineering and exploratory data analysis (EDA) pipeline designed for a multi-region hospital network. In modern healthcare environments, massive operational datasets are often plagued by system-entry corruption, chronological silos, and missing records. 

This project simulates a production-grade ingestion layer, implementing rigorous data validation rules, systemic outlier removal, and statistical missing value repair using **Python (Pandas, NumPy)** and **Jupyter Notebooks**. The final, audited dataset is used to extract high-fidelity business insights regarding regional market performance, department profit margins, and omnichannel patient satisfaction.

---

## 🎯 Core Business Problem Statement
The hospital network's distributed database was facing an administrative visibility crisis driven by three major vulnerabilities:
1. **Data Integrity Failures:** Legacy registration software allowed impossible values to bypass system validation—specifically creating entries with patient ages up to 150 and customer satisfaction scores scaling up to 10 (on an intended 1-5 scale).
2. **Operational Opacity:** Corporate executives lacked a unified, trusted analytical dashboard across the four core medical service categories and regional territories to map true profit vs. revenue performance.
3. **Omnichannel Patient Friction:** Fluctuations in patient satisfaction were observed across intake pathways (Mobile, Online, Offline), but management could not identify whether the bottleneck stemmed from digital UI bugs or backend clinical care delivery.

This pipeline resolves these blind spots by establishing an absolute, clean **"Source of Truth"** for corporate decision-making.

---

## 📊 Dataset Compliance & Architecture
The final processed analytical dataset completely adheres to rigorous data-scale parameters:
* **Row Count:** 96,971 fully sanitized entries (Shattering the target benchmark of $\ge$ 20,000 rows).
* **Column Count:** 25 core operational features (Exceeding the target benchmark of $\ge$ 10 columns).

### Target Core Attributes Tracked:
* **Demographics:** `Customer_ID`, `Name`, `Age`, `Gender`, `City`, `State`, `Region`
* **Operations:** `Date`, `Category` (Medical Department), `SubCategory`, `Channel` (Intake Route), `Status`
* **Financials:** `Quantity`, `Cost`, `Price`, `Discount`, `Revenue`, `Profit`
* **Sentiment:** `Rating` (1-5 Star Scale), `Comments` (Qualitative Feedback)

---

## 🛠️ Data Pipeline Execution Workflow

### Phase 1: Chronological Data Unification (Concatenation)
To mirror real-world healthcare enterprise ingestion where data is partitioned chronologically, the raw transaction ledger ($101,000$ starting rows) was split into historic (Pre-2024) and current (Post-2023) files.
* **Function Used:** `pd.concat(axis=0)` was executed to vertically stack the dataframes into a continuous stream, aligning headers perfectly.
* **Re-indexing:** `.reset_index(drop=True)` was used to wipe the scrambled fragmentary row ID indicators and restore sequential indices.

### Phase 2: Data Sanitation & Repair
* **Timeline Conversion (`pd.to_datetime`):** Plain text representations of transaction dates were transformed into proper chronological timeline objects to enable precise seasonal queries.
* **Geographic Backfilling (`.fillna('Unknown')`):** Over 3,000 empty cells within the patient `City` attribute were preserved by mapping them to an `'Unknown'` flag, protecting downstream financial summaries.
* **Smart Medical Pricing Imputation (`.groupby` + `.transform` + `.median`):** For $3,040$ records missing historical treatment costs, an advanced group-median imputation was engineered. The pipeline grouped transactions by their medical `Category` and replaced the blanks with that specific department’s median price.
* **Discount Neutralization (`.fillna(0)`):** Blank discount percentage cells were structurally assigned a value of `0` to prevent mathematical failure strings during revenue recalculations.
* **Text Formatting (`.str.strip` + `.str.lower`):** Manual string inputs for customer comments were cleansed of accidental whitespace and forced to lower-case, resolving data variance bugs (e.g., standardizing `" Good "`, `"GOOD"`, and `"good"` into `good`).

### Phase 3: Outlier & Anomaly Purge
Rigorous boolean validation boundaries were executed to defend data integrity:
* **Age Ceiling Filter:** Dropped **2,014 records** where a system glitch registered ages between 101 and 150.
* **Survey Baseline Filter:** Eliminated **2,015 records** holding obsolete satisfaction entries that went up to 10 stars, enforcing the standard 1-5 metric.
* **Deduplication (`.drop_duplicates()`):** Audited rows across the entire dataset matrix to remove exact duplicate entries.

---

## 📈 Strategic Business Insights (EDA Highlights)

### 1. Unified Regional Footprint
The programmatic regional revenue evaluation exposed an exact, symmetrical market split: **25.0% revenue contribution** each from the North, South, East, and West zones. This signals exceptionally stable national demand and uniform brand penetration.

### 2. Primary Financial Engines
Medical Services falling under **Category C** serve as the hospital's largest gross revenue driver, followed closely by **Category B**. Network-wide profit margins remain highly optimized, tracking reliably between **32% and 34%** across all clinical divisions.

### 3. Patient Dissatisfaction Root Cause
Cross-tabulation metrics proved that negative or `poor` patient feedback scores are distributed equally across all appointment booking pathways (Mobile App, Online Web Portal, and Offline Walk-ins). Because the friction does not spike on digital platforms, management can conclude that the bottleneck lies in live, backend clinical care delivery rather than digital software bugs.

---

## 🚀 How to Run the Project Locally

### Prerequisites
Ensure you have Python 3.8+ and the following libraries installed:
```bash
pip install pandas numpy matplotlib seaborn jupyter
