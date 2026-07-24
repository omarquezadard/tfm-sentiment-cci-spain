# Sentimiento del Discurso Institucional vs. Índice de Confianza del Consumidor — España (2024–2026)
# Institutional Discourse Sentiment vs. Consumer Confidence Index — Spain (2024–2026)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![Model](https://img.shields.io/badge/NLP-pysentimiento%20RoBERTa--es-teal)](https://github.com/pysentimiento/pysentimiento)
[![Data](https://img.shields.io/badge/Data-Eurostat%20%7C%20X%20API%20v2-orange)](https://ec.europa.eu/eurostat)
[![License](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey)](LICENSE)
[![TFM](https://img.shields.io/badge/TFM-UCJC%20Ciencia%20de%20Datos-1A2C5B)](thesis/)

---

> 🇪🇸 [Español](#-español) · 🇬🇧 [English](#-english)

---

## 🇪🇸 Español

### Descripción

Este proyecto analiza si el **tono del discurso institucional** publicado por el
Ministerio de Economía, Comercio y Empresa (`@_minecogob`) en X (antes Twitter)
guarda relación con la evolución del **Índice de Confianza del Consumidor (CCI)**
en España durante el periodo 2024–2026.

Mediante análisis de sentimiento con PLN sobre 716 tweets originales y métodos
econométricos de series temporales sobre 25 observaciones mensuales, **el estudio
no encuentra una asociación contemporánea ni una relación predictiva robusta y
estadísticamente significativa** entre el sentimiento institucional y la confianza
del consumidor.

### Pregunta de investigación

> *¿El tono de la comunicación institucional del Ministerio se correlaciona con
> — o anticipa — el Índice de Confianza del Consumidor en España?*

### Resultados principales

| Métrica | Valor | Interpretación |
|---|---|---|
| Pearson *r* (lag 0) | 0,147 (p = 0,484) | Positivo débil, no significativo |
| Spearman *r* (lag 0) | 0,200 (p = 0,339) | Positivo débil, no significativo |
| ADF — Sentiment Score | −2,809 (p = 0,057) | Límite de estacionariedad — tratada como I(1) |
| ADF — CCI (niveles) | −2,080 (p = 0,253) | No estacionaria — tratada como I(1) |
| CCF máximo (lag 5) | −0,290 | Por debajo del IC 95% (±0,400) — no significativo |
| Causalidad de Granger (lags 1–3) | p > 0,05 | No se rechaza H₀ de no causalidad |
| Clase dominante de sentimiento | NEU — 83,2% | Coherente con tono informativo institucional |

> **Conclusión:** la ausencia de relación significativa es en sí misma un hallazgo
> relevante. El discurso oficial no refleja ni anticipa de forma estadísticamente
> robusta la percepción de los hogares en el periodo analizado.

### Metodología

```
API X v2                   Eurostat (ei_bsco_m)
     │                            │
     ▼                            ▼
~716 tweets originales       CCI España (SA)
(sin retweets desde API)     25 obs. mensuales
     │
     │ Limpieza Regex (URLs, @menciones)
     ▼
pysentimiento (RoBERTa-es)
  → prob_POS, prob_NEG, prob_NEU
     │
     │ Agregación mensual (resample)
     │ Score = mean(POS) − mean(NEG)
     ▼
Comparación de series (ambas I(1) → primera diferencia)
  ├── ADF (estacionariedad)
  ├── Pearson & Spearman (correlación en nivel)
  ├── CCF (lags 0–5, sobre series diferenciadas)
  └── Causalidad de Granger (lags 1–3)
```

### Estructura del repositorio

```
tfm-sentiment-cci-spain/
├── README.md
├── requirements.txt          # Dependencias directas (versiones mínimas compatibles)
├── requirements-lock.txt     # Versiones aproximadas del entorno del TFM (marzo 2026)
├── .gitignore
├── LICENSE
├── notebook/
│   └── sentiment_cci_spain.ipynb   # Pipeline completo reproducible
├── thesis/
│   └── TFM_Omar_Quezada_Final.pdf  # Documento completo del TFM
└── data/
    ├── README_data.md              # Cómo obtener los datos
    └── cci_spain.csv               # CCI España SA — Eurostat (dominio público) ✅
```

### Reproducibilidad

El repositorio contiene el pipeline completo, pero la reproducción exacta del
corpus depende de la disponibilidad de los datos de X:

- **`data/cci_spain.csv`** — incluido. Datos públicos de Eurostat.
- **`tweets_minecogob.csv`** — no incluido (Términos de la API de X). Ver `data/README_data.md`.

### Instalación

```bash
git clone https://github.com/omarquezadard/tfm-sentiment-cci-spain.git
cd tfm-sentiment-cci-spain
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebook/sentiment_cci_spain.ipynb
```

### Cita

```bibtex
@mastersthesis{quezada2026sentiment,
  author = {Quezada Dalmau, Omar Francisco},
  title  = {Comparativa del Índice de Sentimiento del Consumidor con el
             Análisis de Sentimiento del Discurso con Tweets del
             Ministerio de Comercio en Distintos Momentos},
  school = {Universidad Camilo José Cela},
  year   = {2026},
  month  = {March},
  url    = {https://github.com/omarquezadard/tfm-sentiment-cci-spain}
}
```

### Autor

**Omar Francisco Quezada Dalmau**  
Fundador, SUBROSA · Director, SonDatos  
🇩🇴 República Dominicana  
[GitHub](https://github.com/omarquezadard)

---

## 🇬🇧 English

### Overview

This project explores whether the **tone of institutional discourse** published by
Spain's Ministry of Economy, Trade and Business (`@_minecogob`) on X (formerly Twitter)
is related to the evolution of the **Consumer Confidence Index (CCI)** over the same period.

Using NLP sentiment analysis on 716 original tweets and econometric time-series methods
on 25 monthly observations, **the study finds no statistically significant contemporaneous
or robust predictive relationship** between institutional sentiment and consumer confidence.

### Research Question

> *Does the sentiment tone of the Ministry's institutional communication correlate
> with — or predict — the Consumer Confidence Index in Spain?*

### Key Findings

| Metric | Value | Interpretation |
|---|---|---|
| Pearson *r* (lag 0) | 0.147 (p = 0.484) | Weak positive, not significant |
| Spearman *r* (lag 0) | 0.200 (p = 0.339) | Weak positive, not significant |
| ADF — Sentiment Score | −2.809 (p = 0.057) | Near-boundary — treated as I(1) |
| ADF — CCI (levels) | −2.080 (p = 0.253) | Non-stationary — treated as I(1) |
| CCF max (lag 5) | −0.290 | Below IC 95% (±0.400) — not significant |
| Granger causality (lags 1–3) | p > 0.05 | H₀ of non-causality not rejected |
| Dominant sentiment class | NEU — 83.2% | Consistent with informational tone |

> **Summary:** The absence of a significant relationship is itself a relevant finding.
> The official discourse neither reflects nor anticipates household perception in a
> statistically robust way during the period analysed.

### Methodology

```
X API v2                   Eurostat (ei_bsco_m)
     │                            │
     ▼                            ▼
~716 original tweets         CCI Spain (SA)
(retweets excluded at API)   25 monthly obs.
     │
     │ Regex cleaning (URLs, @mentions)
     ▼
pysentimiento (RoBERTa-es)
  → prob_POS, prob_NEG, prob_NEU
     │
     │ Monthly aggregation (resample)
     │ Score = mean(POS) − mean(NEG)
     ▼
Time-series comparison (both I(1) → first-differenced)
  ├── ADF stationarity test
  ├── Pearson & Spearman correlation (levels)
  ├── CCF (lags 0–5, differenced series)
  └── Granger causality (lags 1–3)
```

### Reproducibility Notice

- **`data/cci_spain.csv`** — included. Public domain data from Eurostat.
- **`tweets_minecogob.csv`** — not included (X Developer Agreement). See `data/README_data.md`.

A new extraction may return a different number of posts as some tweets may have
been deleted or become unavailable since the original study.

### Setup

```bash
git clone https://github.com/omarquezadard/tfm-sentiment-cci-spain.git
cd tfm-sentiment-cci-spain
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebook/sentiment_cci_spain.ipynb
```

### Citation

```bibtex
@mastersthesis{quezada2026sentiment,
  author = {Quezada Dalmau, Omar Francisco},
  title  = {Comparativa del Índice de Sentimiento del Consumidor con el
             Análisis de Sentimiento del Discurso con Tweets del
             Ministerio de Comercio en Distintos Momentos},
  school = {Universidad Camilo José Cela},
  year   = {2026},
  month  = {March},
  url    = {https://github.com/omarquezadard/tfm-sentiment-cci-spain}
}
```

### License

- **Code & notebook:** [MIT License](LICENSE)
- **Thesis document:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Tweet content:** Subject to [X Developer Agreement](https://developer.x.com/en/developer-terms/agreement-and-policy). Raw data not redistributed.

### Author

**Omar Francisco Quezada Dalmau**  
Founder, SUBROSA · Director, SonDatos  
🇩🇴 Dominican Republic  
[GitHub](https://github.com/omarquezadard)
