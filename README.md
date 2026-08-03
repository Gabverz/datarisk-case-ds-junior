<div align="center">

# 💰 Credit Risk Predictor

### *Predicting payment defaults to enable proactive collection strategies.*

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-Model-006699?style=for-the-badge&logo=xgboost&logoColor=white)](https://xgboost.readthedocs.io/)
[![Optuna](https://img.shields.io/badge/Optuna-Hyperparameter_Tuning-007bff?style=for-the-badge&logo=optuna&logoColor=white)](https://optuna.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-Metrics-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/stable/)

</div>

---

## Overview

This project develops a robust predictive model to estimate the probability of payment default for monthly charges, defined as **payments made 5 days or more after the due date**. Leveraging historical payment behavior, customer registration data, and monthly financial information, the solution aims to identify high-risk clients proactively. The entire pipeline, from data exploration and feature engineering to model optimization and interpretation, is documented and reproducible, demonstrating an end-to-end data science approach.

---

## Key Highlights

*   **Target Definition**: Precisely engineered the target variable based on a 5-day payment delay, resulting in approximately 7% of charges classified as default.
*   **Feature Engineering**: Developed critical features such as `TAXA_HISTORICA_INADIMPLENCIA` (historical default rate) and `TEMPO_RELACIONAMENTO_DIAS` (customer relationship duration), which proved highly predictive.
*   **Temporal Validation**: Implemented a temporal train/validation split to simulate real-world scenarios and prevent data leakage, ensuring the model's robustness for future predictions.
*   **Model Optimization**: Utilized Optuna for Bayesian hyperparameter tuning, significantly improving the XGBoost model's AUC-PR from 0.50 to 0.59.
*   **Actionable Insights**: Identified that `TAXA_HISTORICA_INADIMPLENCIA` and regional factors (`CEP_2_DIG`) are the most influential predictors, providing clear guidance for business interventions.
*   **Proactive Strategy**: The final XGBoost model provides continuous probability scores, enabling targeted collection efforts for the highest-risk customers (e.g., those with probabilities above 0.52, representing the top 7% of risk).

---

## Pipeline Architecture

| Stage | What happens |
|---|---|
| **Raw Data** | Four CSV datasets (`base_cadastral`, `base_info`, `base_pagamentos_desenvolvimento`, `base_pagamentos_teste`) |
| **EDA & Data Cleaning** | Type conversion, null handling, duplicate checks, identification of logical inconsistencies, and target variable construction. |
| **Feature Engineering** | Merging datasets, creating temporal, behavioral, and historical features (`NUMERO_COBRANCA_CLIENTE`, `TEMPO_RELACIONAMENTO_DIAS`, `TAXA_HISTORICA_INADIMPLENCIA`), and handling cold-start clients. |
| **Model Training & Validation** | Temporal split (train/validation), initial evaluation of LightGBM, XGBoost, and CatBoost, focusing on AUC-PR. |
| **Hyperparameter Optimization** | Optuna (TPE algorithm) for tuning XGBoost and CatBoost, selecting the best model based on AUC-PR and training efficiency. |
| **Final Model & Predictions** | Application of the optimized XGBoost model to the test set to generate final default probability predictions. |

---

## Challenges & How We Overcame Them

<table>
<tr>
<td width="33%" valign="top">

**⚠️ Data Inconsistencies & Leakage**

Identified 27 records where `DATA_VENCIMENTO` was prior to `DATA_EMISSAO_DOCUMENTO`. Instead of discarding, a `FLAG_REGISTRO_INCONSISTENTE` was created, as these records showed a 96.3% default rate, preserving valuable predictive signal while preventing direct leakage.

</td>
<td width="33%" valign="top">

**🔍 Cold Start Clients**

Encountered 88 new clients in the test set without prior historical data. Addressed this by implementing fallback strategies for behavioral features, such as filling categorical nulls with "Desconhecido" and allowing `TAXA_HISTORICA_INADIMPLENCIA` to remain `NaN` for first-time charges, which XGBoost handles natively.

</td>
<td width="33%" valign="top">

**📍 High-Cardinality Categorical Features & Redundancy**

`DDD` and `CEP_2_DIG` showed strong association (V de Cramér = 0.63). After assessing their low individual predictive importance, `DDD` was removed to reduce redundancy and model complexity, streamlining the feature set.

</td>
</tr>
</table>

---

## Tech Stack

<table>
  <tr>
    <td><b>Core</b></td>
    <td>Python (3.11) · Pandas · NumPy · Scikit-learn</td>
  </tr>
  <tr>
    <td><b>ML & Tuning</b></td>
    <td>XGBoost · CatBoost · Optuna</td>
  </tr>
  <tr>
    <td><b>Data Visualization</b></td>
    <td>Matplotlib · Seaborn</td>
  </tr>
  <tr>
    <td><b>Version Control</b></td>
    <td>Git · GitHub</td>
  </tr>
</table>

---

## Project Structure

```bash
datarisk-case-ds-junior/
│
├── data/
│   ├── base_cadastral.csv
│   ├── base_info.csv
│   ├── base_pagamentos_desenvolvimento.csv
│   └── base_pagamentos_teste.csv
│
├── notebooks/
│   ├── previsao_inadimplencia.ipynb   # Main development notebook (EDA, FE, Modeling, Evaluation)
│   └── relatorio_solucao.ipynb         # Project report and technical decisions
│
├── .gitignore
├── Case DS Júnior 2025.pdf             # Original project instructions
├── README.md
├── requirements.txt                    # Python dependencies
└── submissao_case.csv                  # Final predictions (ID_CLIENTE, SAFRA_REF, PROBABILIDADE_INADIMPLENCIA)
```

---

## Key Results

| Metric | Score (XGBoost, Validation Set) |
|---|---|
| **AUC-PR** | `0.5867` |
| **Recall** (at threshold 0.5) | `0.8372` |
| **Accuracy** (at threshold 0.5) | `0.8219` |

> Model: XGBoost · Tuning: Optuna · Class imbalance: `scale_pos_weight`

**Top predictive features (XGBoost Gain):**
1. `TAXA_HISTORICA_INADIMPLENCIA`
2. `CEP_2_DIG`
3. `MES_COBRANCA`
4. `VALOR_A_PAGAR`
5. `TEMPO_RELACIONAMENTO_DIAS`

---

<div align="center">
<sub>Built with rigor. Documented with care. Shipped with confidence.</sub>
</div>
