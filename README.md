# Personal Desktop Computer Electricity Consumption
### A Full-Year Exploratory Data Analysis of Computing Behaviour

This repository contains the dataset, analysis notebook, and figures for a one-year
exploratory analysis of personal desktop computer electricity consumption measured with
a smart plug energy monitor. Data was recorded across a one-year observation window from
13 March 2025 to 13 March 2026, spanning **366 days**. Of these, **322 days carry
active power readings**; the remaining 44 days correspond to three intentional travel
intervals during which the PC was powered off and unplugged.

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![pandas](https://img.shields.io/badge/pandas-2.0%2B-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.2%2B-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)

| 📅 Period | 📆 Observation Window | 📊 Days with Readings | ⚡ Total Energy | 🏠 Home Days | 🎓 University Days | 📈 Home vs University |
|---|---|---|---|---|---|---|
| Mar 2025 – Mar 2026 | **366 days** | **322** | **494.1 kWh** | **222** | **100** | **+229%** |

---

## 📖 Overview

This project is a comprehensive **Exploratory Data Analysis (EDA)** of one full year of daily
electricity consumption recorded from a smart plug connected exclusively to a personal desktop
computer system — including the PC, GPU, CPU, monitor, and all peripherals on the same strip.
No other household appliances share the circuit.

The central finding is that the power data alone functions as a **high-fidelity behavioural
sensor**: the near-complete bimodal separation between Home Days and University Days means a
single 0.9 kWh energy threshold reconstructs the university visit log with >95% accuracy.

---

## 🗂️ Repository Structure

```
pc-energy-behavior-analysis/
│
├── 📓 pc_energy_analysis_notebook.ipynb   ← Full analysis pipeline (run this)
├── 📄 requirements.txt                    ← Python dependencies
│
├── 📁 data/
│   ├── final_combined_power_data.csv      ← Daily kWh readings (Date, Power)
│   └── univ_dates.txt                     ← University visit dates (YYYY-MM-DD)
│
├── 📁 figures/                            ← Output figures (auto-generated on run)
│
├── 📁 paper/
│   └── PC_Electricity_EDA_Paper.pdf       ← Research paper
│
├── 📄 LICENSE                             ← MIT License (code)
├── 📄 DATA_LICENSE                        ← Data license
└── 📄 README.md
```

---

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/tardigrade1001/pc-energy-behavior-analysis.git
cd pc-energy-behavior-analysis
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the notebook

```bash
jupyter notebook pc_energy_analysis_notebook.ipynb
```

**Run All Cells** — the full pipeline executes in under 60 seconds and generates all
17 figures in `figures/`.

---

## 📁 Data Format

### `data/final_combined_power_data.csv`

```
Date,Power
2025-03-13,0.72
2025-03-14,2.38
...
```

| Column | Type | Description |
|---|---|---|
| `Date` | `YYYY-MM-DD` | Calendar date |
| `Power` | `float` (kWh) | Daily electricity consumption |

### `data/univ_dates.txt`

One ISO date per line. Dates present here → **University Day**; all others → **Home Day**.

---

## 🔬 Methodology

```
Raw CSV → Feature Engineering → Descriptive Statistics
        → Distribution Analysis (KDE, Histograms)
        → K-Means Clustering  (k = 3 behavioural states)
        → Time-Series Smoothing (7-day & 14-day rolling means)
        → Temporal Variability  (14-day rolling std)
        → Weekly & Monthly Pattern Analysis
        → Calendar Heatmaps
        → Cumulative Consumption
```

### Day-Type Labeling

| Day Type | Definition | Count |
|---|---|---|
| 🏠 **Home Day** | Date not in university log | 222 (68.9%) |
| 🎓 **University Day** | Date present in university log | 100 (31.1%) |

### Intentional Data Gaps (Travel)

| 🌏 Destination | Period | Days | Context |
|---|---|---|---|
| Shizuoka, Japan | 1 Apr – 30 Apr 2025 | 30 | Research collaboration |
| Mumbai, India | 8 Oct – 11 Oct 2025 | 4 | Academic conference |
| Jeju, South Korea | 22 Nov – 1 Dec 2025 | 10 | Academic conference |

---

## 📊 Key Results

### Descriptive Statistics

| Metric | All Days | 🏠 Home Days | 🎓 University Days |
|---|---|---|---|
| **N** | 322 | 222 | 100 |
| **Mean (kWh)** | 1.535 | **1.958** | **0.595** |
| **Median (kWh)** | 1.810 | 1.955 | 0.540 |
| **Std Dev** | 0.734 | 0.420 | 0.249 |
| **CV (%)** | 47.8% | **21.5%** | 41.8% |
| **Total (kWh)** | 494.10 | 434.61 (88%) | 59.49 (12%) |

### Derived Metrics

| Metric | Value |
|---|---|
| ⚡ Home vs University daily difference | **+1.363 kWh (+229%)** |
| 🔌 Estimated average active draw | **~245 W** (8 h/day assumption) |
| ⚠️ Peak estimated draw | **~451 W** (29 Jun 2025) |
| 🎓 Estimated active use on University days | **~2–3 h/day** |

### K-Means Behavioural States (k = 3)

| State | Centroid | Days | Share | Interpretation |
|---|---|---|---|---|
| 🟣 **Idle** | 0.598 kWh | 109 | 33.9% | Compressed sessions — nearly co-extensive with University days |
| 🟢 **Moderate** | 1.802 kWh | 126 | 39.1% | Normal home computing session |
| 🔴 **Heavy** | 2.321 kWh | 87 | 27.0% | Extended or intensive home session |

---

## 📓 Notebook Structure

| Section | Contents |
|---|---|
| **1 · Environment Setup** | Imports, directory creation |
| **2 · Global Plotting Config** | `FIG_FORMAT`, style, colour palette, `shade_travel()`, `savefig()` |
| **3 · File Loading & Preparation** | CSV + date-file loading, gap audit |
| **4 · Feature Engineering** | Day-type labels, derived columns, `MONTH_ORDER` |
| **5 · Descriptive Statistics** | Tables 2 & 3, derived metrics, threshold classifier |
| **6 · Distribution Analysis** | KDE parameterisation note |
| **7 · Temporal Analysis** | 7-day & 14-day rolling windows |
| **8 · Clustering Analysis** | K-Means fit, centroid labeling |
| **9 · Figure Generation** | All 17 figures in paper order |
| **Appendix** | Figures 16–17 |

---

## 🖼️ Output Figures

Running the notebook saves the following files to `figures/`:

| File | Paper Figure | Description |
|---|---|---|
| `fig01_timeseries` | Figure 1 | Full-year scatter + 7/14-day moving averages |
| `fig02_monthly_bars` | Figure 2 | Monthly totals + Home/University split |
| `fig03_monthly_trend` | Figure 3 | Monthly average daily consumption trend |
| `fig04_histograms` | Figure 4 | Histograms + KDE (All / Home / University) |
| `fig05_kde_overlap` | Figure 5 | Overlapping KDE: Home vs University |
| `fig06_boxviolin` | Figure 6 | Box plot + violin comparison |
| `fig07_stripplot` | Figure 7 | Strip plot with mean lines |
| `fig08_calendar_heatmap` | Figure 8 | Full-year calendar heatmap |
| `fig09_weekly_patterns` | Figure 9 | Day-of-week box plots + bar chart |
| `fig10_heatmap_dow_month` | Figure 10 | Day-of-week × Month heatmap |
| `fig11_weekday_weekend` | Figure 11 | Weekday vs weekend violin / KDE / bar |
| `fig12_kmeans_states` | Figure 12 | K-Means state time series + count chart |
| `fig13_states_by_daytype` | Figure 13 | Behavioural states by day type + month |
| `fig14_cumulative` | Figure 14 | Cumulative energy consumption curve |
| `fig15_rolling_variability` | Figure 15 | 14-day rolling mean ± std + variability |
| `figA16_day_counts` | Appendix 16 | Monthly Home vs University day counts |
| `figA17_dow_scatter` | Appendix 17 | kWh vs day-of-week scatter |


---

## 🧠 Key Behavioural Insights

**1 · The smart plug as a behavioural sensor.**
The bimodal energy distribution cleanly separates Home and University days. A single 0.9 kWh
threshold achieves >95% classification accuracy — no additional sensors or metadata required.

**2 · Home computing is highly habitual.**
A CV of 21.5% across 222 home days is remarkably low for a year-long behavioural dataset. The
home sessions span research work, hobby projects, gaming, AI image generation workflows, and
creative computing — yet produce a stable daily energy signature across the full year. The
consistency comes from behavioural regularity, not task type.

**3 · University days represent real, compressed usage.**
The PC is switched off at the wall before leaving. The 59.5 kWh on university days (12% of
annual total) reflects two genuine short sessions per day — a brief morning window before
commuting and an evening session after returning — totaling approximately 2–3 active hours.

**4 · Travel gaps are legible directly in the data.**
The April plateau, the October travel dip, and the late-November gap are all visible as flat segments in the cumulative
curve without any annotation needed.

**5 · Peak intensity clusters in early summer.**
The top-10 highest-consumption days concentrate in June–July 2025, immediately after the
month-long Japan absence — consistent with intensive reengagement across deferred home
computing activity after 30 days away from the machine.

---

## 👤 Author

**Uddipan Dasgupta**  
PhD Student · Amity Institute of Nanotechnology
Amity University Kolkata, India  
GitHub: [@tardigrade1001](https://github.com/tardigrade1001)

---

## 🤝 Acknowledgements

The data collection, experimental design, day-type labeling, university attendance log,
and all analysis decisions are entirely my own work, carried out over a full calendar year.
The numbers, interpretations, and the research paper are mine.

The Python implementation — notebook structure, plotting code, and figure styling — was
developed with assistance from [Claude](https://claude.ai) (Anthropic).

---

## 📜 License

Code: [MIT License](LICENSE)  
Data: see [DATA_LICENSE](DATA_LICENSE)

---

*Data collection period: 13 March 2025 – 13 March 2026 · 366-day observation window · 322 days with readings · 494.1 kWh · 17 figures*
