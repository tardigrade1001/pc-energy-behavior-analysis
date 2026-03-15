<div align="center">

# ⚡ PC Electricity Consumption — Annual EDA

**A full-year behavioral study of personal computing energy use,<br>tracked via a dedicated power meter across 322 days.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![pandas](https://img.shields.io/badge/pandas-2.0%2B-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3%2B-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)

<br>

| 📅 Period | 📊 Days Recorded | ⚡ Total Energy | 🏠 Home Days | 🎓 University Days | 📈 Home Premium |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Mar 2025 – Mar 2026 | **322** | **494.1 kWh** | **222** | **100** | **+229%** |

</div>

---

## 📖 Overview

This project is a comprehensive **Exploratory Data Analysis (EDA)** of one year of daily electricity consumption recorded from a dedicated power meter connected exclusively to a personal desktop computer system — including the PC, GPU, CPU, monitor, and all peripherals.

The key insight is that the electricity data alone serves as a **high-fidelity behavioral sensor**: the near-complete bimodal separation between Home Days and University Days means the meter can reconstruct the university visit log with >95% accuracy using a simple energy threshold.

The project includes:
- 📓 **A fully reproducible Jupyter notebook** covering the entire analysis pipeline
- 📄 **A research paper** (PDF) formatted in academic style
- 📊 **19 publication-quality figures** covering distributions, time series, clustering, heatmaps, and more

---

## 🗂️ Repository Structure

```
pc-electricity-eda/
│
├── 📓 pc_energy_analysis_notebook.ipynb   ← Main analysis notebook (run this)
│
├── 📁 data/
│   ├── final_combined_power_data.csv      ← Daily kWh readings (Date, Power)
│   └── univ_dates.txt                     ← University visit dates (YYYY-MM-DD)
│
├── 📁 figures/                            ← All output figures (auto-generated)
│   ├── fig01_timeseries.png
│   ├── fig02_monthly_bars.png
│   └── ...  (19 figures total)
│
├── 📄 PC_Electricity_EDA_Paper.pdf        ← Research paper (academic format)
├── 📄 PC_Electricity_EDA_Report.html      ← Interactive HTML report
└── 📄 README.md
```

---

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/<tardigrade1001>/pc-electricity-eda.git
cd pc-electricity-eda
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

<details>
<summary>Or install manually</summary>

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
```

</details>

### 3. Place your data files

```
data/final_combined_power_data.csv
data/univ_dates.txt
```

### 4. Run the notebook

```bash
jupyter notebook pc_energy_analysis_notebook.ipynb
```

Then **Run All Cells** — the full pipeline executes in under 60 seconds and saves all 19 figures to `figures/`.

---

## 📁 Data Format

### `data/final_combined_power_data.csv`

```csv
Date,Power
2025-03-13,0.72
2025-03-14,2.38
2025-03-15,2.75
...
```

| Column | Type | Description |
|--------|------|-------------|
| `Date` | `YYYY-MM-DD` | Calendar date |
| `Power` | `float` (kWh) | Daily electricity consumption |

### `data/univ_dates.txt`

One date per line (`YYYY-MM-DD`). Any date present in this file is labeled a **University Day**; all other recorded dates are labeled a **Home Day**.

```
dates on which I visited University
2025-03-04
2025-03-10
...
```

---

## 🔬 Methodology

The analysis pipeline follows the structure of the accompanying research paper:

```
Raw Data → Feature Engineering → Descriptive Statistics
        → Distribution Analysis (KDE, Histograms)
        → K-Means Clustering (k=3 behavioral states)
        → Time-Series Smoothing (7-day, 14-day rolling averages)
        → Temporal Variability (rolling std)
        → Weekly & Monthly Patterns
        → Calendar Heatmaps
        → Extreme Day Identification
        → Cumulative Consumption
```

### Day-Type Labeling

| Day Type | Definition | Count |
|----------|-----------|-------|
| 🏠 **Home Day** | Date not in university log | 222 (68.9%) |
| 🎓 **University Day** | Date present in university log | 100 (31.1%) |

### Intentional Data Gaps (Travel)

These periods are **not imputed** — they represent genuine physical absence:

| 🌏 Destination | Period | Days | Context |
|---|---|---|---|
| Shizuoka, Japan 🇯🇵 | 1 Apr – 30 Apr 2025 | 30 | Research collaboration (DST-JSPS bilateral project) |
| Mumbai, India 🇮🇳 | 8 Oct – 11 Oct 2025 | 4 | Academic conference |
| Jeju, South Korea 🇰🇷 | 22 Nov – 1 Dec 2025 | 10 | Academic conference |

---

## 📊 Key Results

### Descriptive Statistics

| Metric | All Days | 🏠 Home Days | 🎓 University Days |
|--------|----------|-------------|------------------|
| **N** | 322 | 222 | 100 |
| **Mean (kWh)** | 1.535 | **1.958** | **0.595** |
| **Median (kWh)** | 1.810 | 1.955 | 0.540 |
| **Std Dev** | 0.734 | 0.420 | 0.249 |
| **CV (%)** | 47.8% | **21.5%** | 41.8% |
| **Total (kWh)** | 494.10 | 434.61 (88%) | 59.49 (12%) |

### Derived Metrics

| Metric | Value |
|--------|-------|
| ⚡ Home energy premium per day | **+1.363 kWh (+229%)** |
| 🔌 Estimated average active draw | **~245 W** (8 h/day assumption) |
| ⚠️ Peak estimated draw | **~451 W** (29 Jun 2025) |
| 🎓 Avg active use on University days | **~2–3 h/day** (morning + evening sessions) |

### K-Means Behavioral States

| State | Centroid | Days | Share | Interpretation |
|-------|----------|------|-------|----------------|
| 🟣 **Idle** | 0.598 kWh | 109 | 33.9% | Abbreviated sessions (≈ University days) |
| 🟢 **Moderate** | 1.802 kWh | 126 | 39.1% | Normal home computing session |
| 🔴 **Heavy** | 2.321 kWh | 87 | 27.0% | Extended or intensive work session |

### Classification Accuracy

A single energy threshold (< 0.9 kWh → University Day, else → Home Day) achieves **>95% classification accuracy**, demonstrating that the meter alone can reconstruct the behavioral log.

---

## 🖼️ Selected Visualizations

<table>
<tr>
<td width="50%">
<b>Full-Year Time Series</b><br>
<sub>Blue = Home · Orange = University · Grey = Travel</sub>
</td>
<td width="50%">
<b>Calendar Heatmap</b><br>
<sub>Green = low · Red = high · ✈ = absent</sub>
</td>
</tr>
<tr>
<td width="50%">
<b>Bimodal KDE Distribution</b><br>
<sub>Near-complete separation between day types</sub>
</td>
<td width="50%">
<b>K-Means Behavioral States</b><br>
<sub>Idle / Moderate / Heavy usage regimes</sub>
</td>
</tr>
<tr>
<td colspan="2">
<b>Day-of-Week × Month Heatmap</b><br>
<sub>September–October dip driven by peak university attendance</sub>
</td>
</tr>
</table>

> All figures are reproducible by running the notebook. See `figures/` for the complete set of 19 plots.

---

## 📓 Notebook Structure

The notebook mirrors the structure of the research paper exactly:

| Section | Description |
|---------|-------------|
| **0 · Setup** | Imports, directories, global plot style, color palette, helper functions |
| **1 · Introduction** | Research questions, measurement scope, travel gaps |
| **2 · Dataset Description** | Data loading, university-date parsing, feature engineering, gap audit |
| **3 · Methodology** | Stats helpers, K-Means fit, monthly aggregation |
| **4 · Results** | All 16 primary figures + statistical tables |
| **5 · Discussion** | Threshold classifier evaluation, summary comparison |
| **6 · Conclusion** | Final printed summary of all key metrics |
| **Appendix** | Supplementary figures, figure inventory |

---

## 📦 Requirements

```
pandas>=2.0
numpy>=1.24
matplotlib>=3.7
seaborn>=0.12
scipy>=1.10
scikit-learn>=1.3
jupyter>=1.0
```

Generate `requirements.txt` with:

```bash
pip freeze | grep -E "pandas|numpy|matplotlib|seaborn|scipy|scikit.learn|jupyter" > requirements.txt
```

---

## 📄 Outputs

Running the notebook produces:

| Output | Description |
|--------|-------------|
| `figures/fig01_timeseries.png` | Full-year scatter + moving averages |
| `figures/fig02_monthly_bars.png` | Monthly totals + Home/Univ split |
| `figures/fig03_monthly_trend.png` | Monthly average trend |
| `figures/fig04_histograms.png` | Histograms + KDE (3 panels) |
| `figures/fig05_kde_overlap.png` | Overlapping KDE: Home vs University |
| `figures/fig06_boxviolin.png` | Box plot + violin comparison |
| `figures/fig07_stripplot.png` | Strip plot with mean lines |
| `figures/fig08_weekly.png` | Weekday box plots + bar chart |
| `figures/fig09_heatmap_dow_month.png` | DOW × Month heatmap |
| `figures/fig10_calendar_heatmap.png` | Full-year calendar heatmap |
| `figures/fig11_kmeans.png` | K-Means state time series |
| `figures/fig12_states.png` | State stacked bars |
| `figures/fig13_extremes.png` | Top 10 high / low days |
| `figures/fig14_dow_scatter.png` | kWh vs DOW scatter |
| `figures/fig15_cumulative.png` | Cumulative consumption curve |
| `figures/fig16_day_counts.png` | Monthly day-type counts |
| `figures/fig17_weekday_weekend.png` | Weekday vs weekend analysis |
| `figures/fig18_rolling.png` | Rolling mean + variability |
| `figures/fig19_monthly_states.png` | Monthly state distribution |

---

## 🧠 Key Behavioral Insights

**1 · The meter is a behavioral sensor.**
The bimodal energy distribution cleanly separates Home and University days, enabling >95% classification accuracy from power data alone — no additional sensors needed.

**2 · Home computing is highly habitual.**
With a coefficient of variation of just 21.5% across 222 home days, daily PC usage is remarkably consistent regardless of day-of-week, month, or season. This stability is driven by behavioral regularity rather than task type — the home sessions span gaming, hobby projects, AI workflows, and occasional research work, yet produce a stable daily energy signature across the full year.

**3 · University days have real but compressed usage.**
The 59.5 kWh consumed on university days (12% of the annual total) does not reflect passive standby — the PC is switched off at the wall before leaving. Instead it represents two short active sessions per day: a brief morning session before commuting and an evening session after returning, totalling approximately 2–3 hours of genuine use.

**4 · Travel gaps are legible in the data.**
The Japan plateau (April 2025), Mumbai dip, and Jeju gap are all directly visible in the raw time series and cumulative curve — natural zero-consumption baselines.

**5 · Peak intensity clusters in early summer.**
The top-10 highest-consumption days concentrate in June–July 2025, immediately after the month-long Japan absence — consistent with an intensive reengagement with home computing across the full range of deferred activities after 30 days away from the machine.

---

## 👤 Author

**Uddipan**  
PhD Candidate · Nanotechnology & Advanced Materials Science  
Amity University Kolkata, West Bengal, India  
GitHub: [@tardigrade1001](https://github.com/tardigrade1001)

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
<sub>Data collection period: 13 March 2025 – 13 March 2026 &nbsp;·&nbsp; 322 days &nbsp;·&nbsp; 494.1 kWh &nbsp;·&nbsp; 19 figures</sub>
</div>
