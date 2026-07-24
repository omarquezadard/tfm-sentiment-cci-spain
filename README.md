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

This project explores whether the **tone of institutional discourse** published by
Spain's Ministry of Economy, Trade and Business (`@_minecogob`) on X (formerly
Twitter) is related to the evolution of the **Consumer Confidence Index (CCI)**
over the same period.

Using NLP sentiment analysis on 716 original tweets and econometric time-series
methods on 25 monthly observations, **the study finds no statistically significant
contemporaneous or lagged association** between institutional sentiment and consumer
confidence. Both series are non-stationary (treated as I(1) after first differencing),
and the cross-correlation function reveals no coefficient that surpasses the 95%
confidence interval across the six lags analysed.

---

## Research Question

> *Does the sentiment tone of the Ministry's institutional communication correlate
> with — or predict — the Consumer Confidence Index in Spain?*

---

## Key Findings

| Metric | Value | Interpretation |
|---|---|---|
| Pearson *r* (lag 0) | 0.147 (p = 0.484) | Weak positive, not significant |
| Spearman *r* (lag 0) | 0.200 (p = 0.339) | Weak positive, not significant |
| ADF — Sentiment Score | −2.809 (p = 0.057) | Near-boundary — treated as I(1) |
| ADF — CCI (levels) | −2.080 (p = 0.253) | Non-stationary — treated as I(1) |
| CCF max absolute value | −0.290 (lag 5) | Below IC 95% (±0.400) — not significant |
| Granger causality (lags 1–3) | p > 0.05 | H₀ of non-causality not rejected |
| Dominant sentiment class | NEU — 83.2% | Consistent with informational tone |

> **Summary:** The study finds no statistically significant contemporaneous association
> or robust predictive relationship between institutional sentiment and consumer
> confidence. Exploratory lagged patterns were observed, but none remained stable
> across the robustness analyses to support a predictive interpretation.

---

## Reproducibility Notice

This repository contains the **complete analysis pipeline**, but exact reproduction
of the original corpus depends on access to X (Twitter) data:

- **CCI Spain** (`data/cci_spain.csv`) — included. Public domain data from Eurostat.
- **Raw Eurostat file** (`ei_bsco_m_linear.csv`) — must be downloaded separately
  (see `data/README_data.md`). Used to regenerate `cci_spain.csv` if needed.
- **Tweets** (`tweets_minecogob.csv`) — **not included** due to X Developer Agreement.
  Must be extracted via API (see `data/README_data.md`). Note that some tweets
  may have been deleted or become unavailable since the original extraction.

> The pipeline is fully reproducible from the CSV files. Re-extracting tweets via
> the API may yield a slightly different corpus depending on current data availability.

---

## Methodology

```
X API v2              Eurostat (ei_bsco_m)
    │                        │
    ▼                        ▼
 ~716 original tweets    CCI Spain (SA)
 (retweets excluded        25 monthly obs.
  at API level)
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
Time-series comparison (both series I(1) → first-differenced)
  ├── ADF stationarity test
  ├── Pearson & Spearman correlation (levels)
  ├── Cross-Correlation Function (CCF, lags 0–5, on Δ series)
  └── Granger causality test (lags 1–3, on Δ series)
```

---

## Repository Structure

```
tfm-sentiment-cci-spain/
│
├── README.md                       # This file
├── requirements.txt                # Direct dependencies (minimum compatible versions)
├── requirements-lock.txt           # Exact versions used to produce the results
├── .gitignore
├── LICENSE
│
├── notebook/
│   └── sentiment_cci_spain.ipynb   # Full reproducible pipeline
│
├── thesis/
│   └── TFM_Omar_Quezada_Final.pdf  # Full thesis document
│
└── data/
    ├── README_data.md              # How to obtain the raw data
    └── cci_spain.csv               # CCI Spain SA (Eurostat, public domain) ✅ included
```

---

## Setup & Reproducibility

### 1. Clone the repository

```bash
git clone https://github.com/omarquezadard/tfm-sentiment-cci-spain.git
cd tfm-sentiment-cci-spain
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Obtain the tweet data

Follow instructions in [`data/README_data.md`](data/README_data.md).
Set your Bearer Token as an environment variable:

```bash
export X_BEARER_TOKEN="your_token_here"
```

### 4. Run the notebook

```bash
jupyter notebook notebook/sentiment_cci_spain.ipynb
```

Set `RUN_EXTRACTION = True` to re-run tweet extraction, or `False` to use
your local CSV and skip directly to the analysis.

---

## Data Sources

| Dataset | Source | License |
|---|---|---|
| Consumer Confidence Indicator — Spain | [Eurostat `ei_bsco_m`](https://ec.europa.eu/eurostat/databrowser/view/ei_bsco_m) | Open data (CC BY 4.0) |
| Tweets — `@_minecogob` | X API v2 (timeline endpoint) | X Developer Agreement |

---

## Dependencies

See `requirements.txt` for direct dependencies and `requirements-lock.txt` for
the exact versions used to produce the results in the thesis.

| Library | Purpose |
|---|---|
| `pysentimiento` | Sentiment analysis (RoBERTa-es) |
| `transformers` | HuggingFace transformer models |
| `pandas` | Data manipulation & resampling |
| `statsmodels` | ADF, CCF, Granger tests |
| `scipy` | Pearson & Spearman correlations |
| `matplotlib` / `seaborn` | Visualisation |
| `wordcloud` | Word cloud generation |
| `requests` | X API v2 calls |

---

## Citation

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
  url       = {https://github.com/omarquezadard/tfm-sentiment-cci-spain}
}
```

---

## License

- **Code & notebook:** [MIT License](LICENSE)
- **Thesis document:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Tweet content:** Subject to [X Developer Agreement](https://developer.x.com/en/developer-terms/agreement-and-policy). Raw data not redistributed.

---

## Contact

**Omar Francisco Quezada Dalmau**  
Founder, SUBROSA · Director, SonDatos  
🇩🇴 Dominican Republic  
[GitHub](https://github.com/omarquezadard)
