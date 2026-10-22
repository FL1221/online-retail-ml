# Customer segmentation and churn prediction — Online Retail II

*[Wersja polska](README.md)*

Source code for the MSc thesis *Application of machine learning methods
to the analysis of customer purchasing behaviour in electronic commerce*
(Gdańsk University of Technology, 2026).

The analysis covers two tasks on the **Online Retail II** transactional
dataset (801,291 transactions, 5,859 customers, 2009–2011):

- **customer segmentation** using clustering methods ($k$-means, DBSCAN)
  on RFM features extended by five behavioural features,
- **customer churn prediction** (logistic regression, random forest)
  together with feature importance analysis based on SHAP values.

## Input data

This repository **does not contain the data** — the raw file is about
45 MB and the cleaned dataset about 78 MB.

Download `online_retail_II.xlsx` from the UCI Machine Learning Repository:

> Chen, D. *Online Retail II*. UCI Machine Learning Repository, 2019.
> https://doi.org/10.24432/C5CG6D

Place the file in the repository root, next to the notebooks.

## Running

```bash
pip install -r requirements.txt
jupyter lab
```

The notebooks must be run **in the given order** — each one uses files
written by the previous ones:

| Notebook | Scope | Writes |
|---|---|---|
| `01_eda.ipynb` | loading the raw data, exploratory analysis, cleaning | `online_retail_clean.csv` |
| `02_features.ipynb` | RFM model, five behavioural features, log transformation and standardisation | `rfm_features.csv`, `rfm_scaled.csv` |
| `03_segmentacja.ipynb` | $k$-means, DBSCAN, PCA projection, figures for ch. 6 | `cluster_labels.csv` |
| `04_churn.ipynb` | churn definition, both classifiers, SHAP values, diagnostic analyses | `churn_predictions.csv`, `churn_results.csv`, `churn_diagnostyka.csv` |

The notebooks are stored **together with their outputs**, so the code and
the resulting numbers can be inspected without running anything.

## Reproducibility

All random procedures use a fixed seed (`RANDOM_STATE = 42`): the
train/test split, $k$-means initialisation, the random forest, and the
sample of observations used to compute SHAP values. Re-running the
notebooks with the library versions listed in `requirements.txt`
reproduces the numbers reported in the thesis.

## Result files included

Two result files are included because they correspond directly to tables
in the thesis:

- `churn_results.csv` — metrics of both models on the test set,
- `churn_diagnostyka.csv` — evaluation variants that separate the
  model's predictive ability from the definitional dependency between
  the *Recency* feature and the target variable.

## Methodological note

The features used for churn prediction were computed **exclusively from
transactions preceding the adopted cut-off date** (2011-09-01),
independently of the features used for segmentation, which were computed
over the full observation period. The two feature pipelines are
deliberately separate: shared features would mean using information
about the customer's future behaviour.
