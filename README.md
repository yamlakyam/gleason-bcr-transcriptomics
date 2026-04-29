# INFO-B528 – Project: Gleason–BCR Transcriptomic Analysis

**Author:** Yamlak Bogale  
**Date:** April 2026  

---

## Languages, Tools, and Packages

### Language
- Python (v3.9+)

### Packages
- pandas, numpy – data handling and numerical operations  
- scipy – statistical analysis (Spearman correlation, t-test)  
- lifelines – survival analysis (Cox regression, Kaplan–Meier)  
- scikit-learn – machine learning models and evaluation  
- xgboost – gradient boosting model  
- matplotlib, seaborn – visualization  

### Tools
- Jupyter Notebook  

---

## Program Overview

This project performs an integrated analysis of **prostate cancer transcriptomic data** to study the relationship between:

- **Gleason grade (tumor progression)**
- **Biochemical recurrence (BCR)**

The goal is to identify a gene signature that captures both biological progression and clinical outcome.

The workflow consists of:

1. **Data Preprocessing**
   - Load RNA-seq data from TCGA-PRAD  
   - Clean gene names and aggregate duplicates  
   - Apply log transformation and variance filtering  

2. **Gleason-Based Analysis**
   - Compute Gleason grades from clinical data  
   - Identify genes with **monotonic expression trends** across grades  
     (genes that consistently increase or decrease with tumor aggressiveness)

3. **BCR-Based Analysis**
   - Construct survival dataset (time + event)  
   - Apply Cox proportional hazards regression  
   - Select genes significantly associated with recurrence (p < 0.05)

4. **Integration (Hybrid Gene Selection)**
   - Intersect Gleason-associated and BCR-associated genes  
   - Result: hybrid gene set capturing both progression and recurrence  

5. **Feature Reduction**
   - Apply LASSO Cox regression  
   - Reduce feature set to a compact gene signature  

6. **Modeling and Evaluation**
   - Train classification models (Logistic Regression, Random Forest, XGBoost)  
   - Evaluate using cross-validation metrics (AUC, precision, recall, F1-score)  
   - Perform survival analysis using Kaplan–Meier curves  

---

## Inputs

| File | Description |
|------|-------------|
| `Data/PRADRN~1.TXT` | RNA-seq expression data (TCGA-PRAD) |
| `Data/Clinical_data/...` | Clinical dataset containing Gleason and survival information |

---

## Parameters

- **Variance filter:** genes with variance ≥ 1 retained  
- **Monotonic threshold:** |Spearman correlation| > 0.7, p < 0.05  
- **Cox significance:** p < 0.05  
- **LASSO:** L1-penalized Cox regression  
- **Cross-validation:** 5-fold  

---

## Outputs

### Figures

| Figure | Description |
|--------|-------------|
| Volcano plot | Differential expression between BCR+ and BCR− groups |
| Boxplots | Gene expression differences by recurrence status |
| Risk vs Gleason plot | Relationship between tumor grade and predicted risk |
| Kaplan–Meier curve | Survival separation between high-risk and low-risk groups |

---

### Tables

| Table | Description |
|-------|-------------|
| Model performance table | AUC, precision, recall, and F1-score for all models |

---

## Results Summary

- Monotonic genes (Gleason-related): **2095**  
- Cox genes (BCR-related): **918**  
- Hybrid genes (intersection): **677**  
- Final gene set (after LASSO): **32**  

### Survival Analysis

- Log-rank p-value: **1.1 × 10⁻¹⁵**  
- Hazard Ratio: **16.09**  
- Median survival (high-risk): **1328 days**  

### Model Performance

| Model | AUC |
|------|----|
| Logistic Regression | 0.818 |
| Random Forest | 0.790 |
| XGBoost | 0.813 |

---

## Interpretation

- Genes associated with tumor progression also contribute to recurrence risk  
- Risk score increases consistently with Gleason grade  
- Hybrid gene set improves prediction compared to single-method approaches  
- Biological functions include proliferation, immune response, and tumor invasion  

---

## Key Assumptions

- Gene expression reflects underlying tumor biology  
- Gleason grade is a valid proxy for tumor progression  
- Cox model assumptions (proportional hazards) hold  
- Data preprocessing does not introduce bias  

---

## Generative AI Disclosure

Generative AI tools were used to assist in structuring documentation. All analysis, code, and interpretations were reviewed and validated by the author.