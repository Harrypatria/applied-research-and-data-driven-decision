# Applied Research and Data-Driven Decision

> **A/B testing, causal reasoning, and statistical inference in business practice.**
> Two Indonesian retail case studies, from raw data to managerial decision.

<p align="left">
  <img alt="Python" src="https://img.shields.io/badge/Python-3.10%2B-3B82F6?style=flat-square&logo=python&logoColor=white">
  <img alt="Statsmodels" src="https://img.shields.io/badge/statsmodels-OLS-6366F1?style=flat-square">
  <img alt="SciPy" src="https://img.shields.io/badge/SciPy-Welch%20t--test-8B5CF6?style=flat-square">
  <img alt="Pandas" src="https://img.shields.io/badge/pandas-panel%20data-0EA5E9?style=flat-square">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-1E3A8A?style=flat-square">
</p>

**Author.** Dr. Harry Patria, Chief Data AI at Patria and Co.
**Date.** July 2026
**Institution.** Patria and Co., Jakarta, Singapore, Aberdeen

---

## Table of Contents

- [Overview](#overview)
- [What is inside](#what-is-inside)
- [Quick start](#quick-start)
- [Philosophical scaffold](#philosophical-scaffold)
- [Case 1, Kopi Susu Aren promo](#case-1-kopi-susu-aren-promo)
- [Case 2, Indomie Goreng demand](#case-2-indomie-goreng-demand)
- [Methodology at a glance](#methodology-at-a-glance)
- [Reproducibility](#reproducibility)
- [Repository structure](#repository-structure)
- [Requirements](#requirements)
- [Citation](#citation)
- [License](#license)

---

## Overview

This repository is a compact teaching and practice kit for **applied research and data-driven decision making** in an Indonesian retail context. It shows how to move from a business question to a defensible decision in six stages: question, design, data, analysis, decision, learn.

The kit contains two case studies. Case 1 is a classic randomized **A/B test**. Case 2 is a **multivariate OLS regression** on a panel of outlet-day observations, with a real identification story about breaking collinearity between promo and price.

Every claim in the deck is reproduced in code, and every number in the code is exposed as an editable formula in the Excel companion. Change one input, everything downstream updates.

---

## What is inside

| File | Purpose |
|---|---|
| `Applied_Research_Case_Studies.ipynb` | Colab-ready Jupyter notebook, both case studies end to end in Python |
| `Applied_Research_Case_Studies.xlsx` | 6-sheet workbook, 2,049 live formulas, zero errors, editable by clients |
| `Applied_Research_Deck.pptx` | 9-slide consulting deck, Patria and Co. brand palette |
| `Synopsis_Outline_Pembelajaran.md` | One-page learning outline in Bahasa Indonesia |
| `README.md` | This file |

---

## Quick start

### Option 1, Google Colab, zero install

Click the badge below to open the notebook in Colab. All packages install on first run.

<p align="left">
  <a href="https://colab.research.google.com/github/harrypatria/applied-research-data-driven-decision/blob/main/Applied_Research_Case_Studies.ipynb">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab">
  </a>
</p>

### Option 2, run locally

```bash
git clone https://github.com/harrypatria/applied-research-data-driven-decision.git
cd applied-research-data-driven-decision

pip install -r requirements.txt
jupyter notebook Applied_Research_Case_Studies.ipynb
```

### Option 3, review the deliverables without running code

Open `Applied_Research_Deck.pptx` in PowerPoint or Keynote, and open `Applied_Research_Case_Studies.xlsx` in Excel or Google Sheets. Both are fully self contained.

---

## Philosophical scaffold

Every model rests on assumptions. The kit makes five anchors explicit before any code runs:

| Anchor | Guiding question | Landing in the analysis |
|---|---|---|
| **Ontology** | What is real | Sales, prices, preferences exist as measurable events |
| **Epistemology** | How do we know | Truth from randomization and inference, not intuition |
| **Methodology** | How do we study | A/B testing for causal claims, regression for context |
| **Axiology** | What is valuable | Customer welfare, staff workload, margin, brand equity |
| **Deontology** | What is right | Transparent framing, no participant manipulation |

---

## Case 1, Kopi Susu Aren promo

### Business question

Does a 20 percent price promo lift daily cups sold for Kopi Susu Aren in Kopi Kenangan style outlets in Jabodetabek?

### Design

- **Product.** Kopi Susu Aren, 22oz
- **Price contrast.** Regular Rp 22,000 vs promo Rp 17,600
- **Units.** 60 outlets, 30 treatment and 30 control
- **Window.** 14 days
- **Randomization.** Stratified by region (Jakarta, Bekasi, Depok, Tangerang, Bogor)
- **Metric.** Cups sold per outlet per day
- **Alpha.** 0.05, two-sided
- **Power.** 0.80

### Statistical test

Welch two-sample t-test with Satterthwaite degrees of freedom, Cohen d for effect size.

### Result

| Quantity | Value |
|---|---|
| Mean control | 179.2 cups per day |
| Mean treatment | 217.3 cups per day |
| Mean difference | +38.0 cups per day |
| Welch t | 7.36 |
| Degrees of freedom | 57.9 |
| p value | 7.5e-10 |
| 95 percent CI | [27.7, 48.4] |
| Cohen d | 1.90 (large) |

**Decision.** Reject H₀. Lift is large, precise, and revenue positive after netting the discount. Roll out.

---

## Case 2, Indomie Goreng demand

### Business question

How does a promo affect daily units sold at Indomaret once we control for price, rainfall, payday, competitor promo, and weekend?

### Design

- **Product.** Indomie Goreng, single SKU
- **Panel.** 30 outlets across Jabodetabek, 60 consecutive days
- **Observations.** 1,800 outlet-day rows
- **Variables.** `units_sold` (y), `promo`, `price_rp`, `rainfall_mm`, `payday_flag`, `competitor_promo`, `weekend`

### Model

$$\text{units\_sold}_{it} = \alpha + \beta_1 \text{promo} + \beta_2 \text{price\_rp} + \beta_3 \text{rainfall} + \beta_4 \text{payday} + \beta_5 \text{competitor} + \beta_6 \text{weekend} + \varepsilon_{it}$$

### Identification note

Promo and price would be perfectly collinear if every promo used the same discount. The data generator **varies outlet base price** (Rp 3,300 to Rp 3,800) and **varies promo depth** (10 to 25 percent), so `promo` and `price_rp` are correlated but not collinear. This mirrors real world franchisee pricing.

### Result

| Predictor | β | SE | t | p | sig |
|---|---|---|---|---|---|
| Intercept | 110.0 | 3.53 | 31.2 | < 0.001 | *** |
| promo | +18.35 | 0.73 | 25.3 | < 0.001 | *** |
| price_rp | −0.0142 | 0.001 | −14.5 | < 0.001 | *** |
| rainfall_mm | −0.237 | 0.027 | −8.9 | < 0.001 | *** |
| payday_flag | +8.75 | 0.38 | 23.1 | < 0.001 | *** |
| competitor_promo | −5.76 | 0.46 | −12.5 | < 0.001 | *** |
| weekend | +8.87 | 0.39 | 22.6 | < 0.001 | *** |

- **R²** = 0.79, **Adj R²** = 0.79, **F** = 1,140
- **Point price elasticity at means:** ε ≈ −0.70, **inelastic**
- **ROI, national rollout (20,000 outlets, 30 days):** net incremental gross margin ≈ **Rp 26.4 miliar**

**Decision.** Roll out, anchor promo on payday windows, avoid overlaps with competitor promo, retest quarterly.

---

## Methodology at a glance

```
┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐
│  Question  │→ │   Design   │→ │    Data    │→ │  Analysis  │→ │  Decision  │→ │   Learn    │
└────────────┘  └────────────┘  └────────────┘  └────────────┘  └────────────┘  └────────────┘
     ▲                                                                                │
     └────────────────────────── continuous loop ─────────────────────────────────────┘
```

Six stages. Every decision produces the next question.

---

## Reproducibility

- Random seed is fixed at `20260704`.
- The notebook and the Excel workbook use the **same data generating process**, so numbers align within rounding.
- To regenerate with a different seed, pass `np.random.default_rng(seed)` into `generate_case1` and `generate_case2`.
- To export the generated datasets:

```python
case1.to_csv("case1_kopi_susu.csv", index=False)
case2.to_csv("case2_indomie.csv", index=False)
```

- Formula errors in the Excel workbook, checked with `python scripts/recalc.py`: **0 errors across 2,049 formulas**.

---

## Repository structure

```
applied-research-data-driven-decision/
├── Applied_Research_Case_Studies.ipynb   # Colab notebook, both cases in Python
├── Applied_Research_Case_Studies.xlsx    # 6-sheet workbook with live formulas
├── Applied_Research_Deck.pptx            # 9-slide consulting deck
├── Synopsis_Outline_Pembelajaran.md      # Bahasa Indonesia learning outline
├── requirements.txt                      # Python dependencies
├── LICENSE                               # MIT
└── README.md                             # this file
```

---

## Requirements

```
numpy >= 1.24
pandas >= 2.0
scipy >= 1.10
statsmodels >= 0.14
matplotlib >= 3.7
seaborn >= 0.12
jupyter
```

Install with:

```bash
pip install -r requirements.txt
```

---

## Citation

If you use this material in teaching, research, or client work, please cite:

```bibtex
@misc{patria2026applied,
  author       = {Patria, Harry},
  title        = {Applied Research and Data-Driven Decision, A/B Testing
                  and Causal Reasoning in Indonesian Retail},
  year         = {2026},
  publisher    = {Patria and Co.},
  howpublished = {\url{https://github.com/harrypatria/applied-research-data-driven-decision}},
}
```

---

## License

Released under the **MIT License**. See `LICENSE` for the full terms.

Case names (Kopi Kenangan, Indomaret, Alfamart, Indomie) are used purely for narrative realism. All figures and data are **synthetic** and generated in code from a documented data generating process. No real corporate data is included in this repository.

---

<p align="center">
  <sub>Prepared with care by <a href="https://patria.co.id">Patria and Co.</a> · Jakarta · Singapore · Aberdeen · 2026</sub>
</p>
