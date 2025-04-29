# Machine Learning for Public Health: Predicting COVID-19 Vaccination Uptake and Test Positivity in the U.S.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.9%2B-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-1.13%2B-red)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.1%2B-orange)
![LightGBM](https://img.shields.io/badge/LightGBM-3.3%2B-brightgreen)
![XGBoost](https://img.shields.io/badge/XGBoost-1.6%2B-blueviolet)
![Stars](https://img.shields.io/github/stars/your-org/your-repo?style=social)

**Team #36:** Fanxing Bu, Xiao Yan, Diego Bermudez, Karla Mercado  
**Course:** 95-828: Machine Learning for Problem Solving  
**Date:** April 27th, 2025

---

## 1. Introduction

During the COVID-19 pandemic, understanding factors influencing vaccine uptake and virus spread was critical for public health interventions. Significant variations in vaccination acceptance and test positivity rates were observed across U.S. communities. This project, undertaken in collaboration with the CDC, aimed to leverage machine learning to predict county-level COVID-19 vaccination uptake and test positivity rates using self-reported behavioral and belief indicators.

**Goals:**
- Predict vaccine uptake based on reported behaviors and beliefs.
- Predict COVID-19 test positivity rates.

---

## 2. Data

### 2.1 Dataset Description

We used data from the COVID-19 Trends and Impact Survey (CTIS), conducted by the Delphi Group at Carnegie Mellon University. The dataset contains 25,627 instances across 687 U.S. counties from Jan 7 to Feb 12, 2021.

### 2.2 Features

- **COVID Activity Indicators:** Symptom prevalence, testing rates, official case incidence, vaccination rates.
- **Behavioral Indicators:** Mask usage, public activity (e.g. work, events, transit), social contacts.
- **Belief Indicators:** Worry about illness, likelihood to vaccinate, trust in different authorities.
- **Other:** Date, county code.

### 2.3 Target Variables

- **Vaccine Uptake:** `smoothed_wcovid_vaccinated`
- **Test Positivity Rate:** `smoothed_wtested_positive_14d`

---

## 3. Data Analysis and Preprocessing

### 3.1 Handling Missing Data

Missing feature values were imputed using forward and backward fill. Missing targets (especially test positivity) were addressed with pseudo-labeling.

### 3.2 Outlier Detection

DBSCAN was used to detect and remove outliers post-imputation.

### 3.3 Feature Exploration and Selection

EDA revealed important correlations and guided feature selection. All features were retained with reliance on model robustness.

### 3.4 Feature Scaling

MinMaxScaler was applied to features and vaccine uptake target, scaling values to [0, 1].

---

## 4. Methodology

### 4.1 Modeling Approach

We modeled $y_{t+1} = f(BX_{t} + Wy_{t} + c)$, using current features and past targets. Feature-based prediction was prioritized due to temporal sparsity.

### 4.2 Data Splitting

- **Traditional Models:** Random train/test split, K-fold CV.
- **Temporal Models:** TimeSeriesSplit by county to avoid leakage.

### 4.3 Models Evaluated

**Vaccine Uptake:**
- Lasso Regression (baseline)
- Gradient Boosting (GBR), XGBoost, LightGBM
- RNN

**Test Positivity Rate:**
- Linear & Ridge Regression (baselines)
- Random Forest, XGBoost, MLP
- LSTM, Transformer (for pseudo-labeling)

### 4.4 Evaluation Metrics

Metrics: R², MSE, RMSE, MAE.

### 4.5 Pseudo-Labeling

LSTM and Transformer models were used to infer missing test positivity labels, enriching the training set.

---

## 5. Results

### 5.1 Model Performance Summary

**Vaccine Uptake**
| Model            | Test RMSE | Test R²  | CV R² (mean ± std) |
|------------------|-----------|----------|--------------------|
| Lasso Regression | 0.2223    | 0.2227   | 0.4545 ± 0.0187    |
| XGBoost          | 6.059     | -0.7003  | -0.7004 ± 0.3571   |
| LightGBM         | 6.000     | -0.6672  | -0.6673 ± 0.3611   |
| GBR              | 0.2203    | 0.4979   | 0.4631 ± 0.0214    |
| RNN              | 2.6651    | 0.8328   | 0.6112 ± 0.1019    |

**Test Positivity Rates**
| Model            | Test RMSE | Test R²  | CV R² (mean ± std) |
|------------------|-----------|----------|--------------------|
| Linear Regression| 4.249     | 0.665    | 0.6296 ± 0.0183    |
| Ridge Regression | 4.252     | 0.665    | 0.6298 ± 0.0180    |
| Random Forest    | 2.700     | 0.865    | 0.8139 ± 0.0134    |
| XGBoost          | 2.530     | 0.881    | 0.8381 ± 0.0088    |
| Neural Network   | 2.292     | 0.903    | 0.8785 ± 0.0147    |

### 5.2 Key Findings

- RNN and MLP were the best models for vaccine uptake and test positivity, respectively.
- Transformers highlighted 5–6 day lag effects, aligning with COVID-19 incubation.
- Pseudo-labeling improved coverage but slightly reduced R² due to noise.
- Political trust, exposure behaviors, and health risk perception were critical features.

---

## 6. Conclusions and Policy Recommendations

1. **Integrate Behavior and Vaccination Messaging:** Highlight the synergy between protective behaviors and vaccines.
2. **Target High-Exposure Settings:** Focus resources on public transit and event-heavy locations.
3. **Enhance Health Risk Communication:** Use clear messaging about personal and community risks.
4. **Address Political Trust:** Engage bipartisan messengers to promote vaccination.
5. **Sustain Outreach Over Time:** Leverage time-sensitive behavioral trends.

---

## 7. References

For full citations and figures, see the report: `Project_Report.docx`.
