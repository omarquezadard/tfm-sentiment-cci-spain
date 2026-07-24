# Institutional Discourse Sentiment vs. Consumer Confidence Index in Spain (2024–2026)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![Model](https://img.shields.io/badge/NLP-pysentimiento%20RoBERTa--es-teal)](https://github.com/pysentimiento/pysentimiento)
[![Data](https://img.shields.io/badge/Data-Eurostat%20%7C%20X%20API%20v2-orange)](https://ec.europa.eu/eurostat)
[![License](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey)](LICENSE)
[![TFM](https://img.shields.io/badge/Master's%20Thesis-UCJC%20Data%20Science-1A2C5B)](thesis/)

> **Master's Thesis (TFM)** — *Máster Universitario en Ciencia de Datos*  
> Universidad Camilo José Cela · March 2026  
> **Author:** Omar Francisco Quezada Dalmau · **Supervisor:** Ana Lazcano de Rojas

---

## Overview

This project explores whether the **tone of institutional discourse** published by Spain's Ministry of Economy, Trade and Business (`@_minecogob`) on X (formerly Twitter) is related to the evolution of the **Consumer Confidence Index (CCI)** over the same period.

Using NLP sentiment analysis on 716 original tweets and econometric time-series methods on 25 monthly observations, the study finds **no statistically significant contemporaneous correlation**, but surfaces an exploratory **inverse lag pattern at lag 4** in the cross-correlation function — a gap between official narrative and household perception that warrants further investigation.

---

## Research Question

> *Does the sentiment tone of the Ministry's institutional communication correlate with — or predict — the Consumer Confidence Index in Spain?*

---

## Key Findings

| Metric | Value | Interpretation |
|---|---|---|
| Pearson *r* (lag 0) | 0.147 (p = 0.484) | Weak positive, not significant |
| Spearman *r* (lag 0) | 0.200 (p = 0.339) | Weak positive, not significant |
| CCF lag 4 | −0.387 | Exploratory inverse pattern |
| Granger causality (lag 1–3) | p = 0.36–0.87 | H₀ of non-causality not rejected |
| ADF — Sentiment Score | −4.308 (p = 0.0004) | Stationary ✓ |
| ADF — CCI | −0.811 (p = 0.816) | Non-stationary ✗ |
| Dominant sentiment class | NEU — 83.2% | Consistent with informational tone |

---

## Methodology

```
X API v2         Eurostat (ei_bsco_m)
    │                     │
    ▼                     ▼
 1,100 tweets        CCI Spain (SA)
    │             25 monthly obs.
    │ filter RT
    ▼
 716 original tweets
    │
    │ Regex cleaning
    │ (URLs, @mentions, ctrl chars)
    ▼
pysentimiento (RoBERTa-es)
  → prob_POS, prob_NEG, prob_NEU
    │
    │ Monthly aggregation
    │ Score = mean(POS) − mean(NEG)
    ▼
Time-series comparison
  ├── ADF stationarity test
  ├── Pearson & Spearman correlation
  ├── Cross-Correlation Function (CCF, lags 0–6)
  └── Granger causality test (lags 1–3)
```

---

## Repository Structure

```
tfm-sentiment-cci-spain/
│
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── .gitignore
├── LICENSE
│
├── notebook/
│   └── Scrapping_TFM_final.ipynb   # Full reproducible pipeline
│
├── thesis/
│   └── TFM_Omar_Quezada_Final.pdf  # Full thesis document
│
└── data/
    ├── README_data.md           # How to obtain the raw data
    └── cci_spain.csv            # CCI Spain (Eurostat, public domain)
```

> **Note on tweets:** Raw tweet data cannot be redistributed under X Developer Agreement. See [`data/README_data.md`](data/README_data.md) for instructions on how to reproduce the extraction using the official API.

---

## Setup & Reproducibility

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/tfm-sentiment-cci-spain.git
cd tfm-sentiment-cci-spain
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Obtain the data

- **CCI Spain:** already included in `data/cci_spain.csv` (Eurostat, public domain).
- **Tweets:** follow instructions in [`data/README_data.md`](data/README_data.md) to extract them via the X API v2. Set your Bearer Token as an environment variable:

```bash
export X_BEARER_TOKEN="your_token_here"
```

### 4. Run the notebook

```bash
jupyter notebook notebook/sentiment_cci_spain.ipynb
```

Set `RUN_EXTRACTION = True` if you want to re-run the tweet extraction, or `False` to skip directly to the analysis using your local CSV.

---

## Data Sources

| Dataset | Source | License |
|---|---|---|
| Consumer Confidence Indicator — Spain | [Eurostat `ei_bsco_m`](https://ec.europa.eu/eurostat/databrowser/view/ei_bsco_m) | Open data (CC BY 4.0) |
| Tweets — `@_minecogob` | X API v2 (timeline endpoint) | X Developer Agreement |

---

## Dependencies

Key libraries (see `requirements.txt` for full pinned versions):

| Library | Purpose |
|---|---|
| `pysentimiento` | Sentiment analysis (RoBERTa-es) |
| `transformers` | HuggingFace transformer models |
| `pandas` | Data manipulation & resampling |
| `statsmodels` | ADF, CCF, Granger tests |
| `scipy` | Pearson & Spearman correlations |
| `matplotlib` / `seaborn` | Visualisation |
| `requests` | X API v2 calls |

---

## Citation

If you use this work or pipeline, please cite:

```bibtex
@mastersthesis{quezada2026sentiment,
  author    = {Quezada Dalmau, Omar Francisco},
  title     = {Comparativa del Índice de Sentimiento del Consumidor con el
               Análisis de Sentimiento del Discurso con Tweets del
               Ministerio de Comercio en Distintos Momentos},
  school    = {Universidad Camilo José Cela},
  year      = {2026},
  month     = {March},
  type      = {Trabajo Fin de Máster},
  url       = {https://github.com/<your-username>/tfm-sentiment-cci-spain}
}
```

---

## License

- **Code & notebook:** [MIT License](LICENSE)
- **Thesis document:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to share and adapt with attribution.
- **Tweet content:** Subject to [X Developer Agreement](https://developer.x.com/en/developer-terms/agreement-and-policy). Raw data not redistributed.

---

## Contact

**Omar Francisco Quezada Dalmau**  
Founder, SUBROSA · Director, SonDatos  
🇩🇴 Dominican Republic  
[GitHub](https://github.com/<your-username>) · [LinkedIn](https://linkedin.com/in/<your-profile>)
