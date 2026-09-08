# 🦠 COVID-19 Risk Level Classifier

## 📌 Overview

An end-to-end COVID-19 risk classification system built on the CDC COVID-19 Case Surveillance Public Use Dataset. The project classifies cases into three risk levels — 🟢 Low, 🟡 Medium, and 🔴 High — using two classifiers built from scratch: a rule-based model and a Bayesian model. No pre-built ML libraries (scikit-learn, TensorFlow, etc.) were used.

## 🎯 Objective

- Work with a large-scale real-world healthcare dataset
- Build a representative sample via stratified sampling
- Preprocess data and select relevant features
- Engineer a risk-level target variable
- Implement rule-based and Bayesian classifiers from scratch
- Compare model performance and visualize results
- Support manual and interactive patient-input testing

## 📊 Dataset

**Source:** [CDC COVID-19 Case Surveillance Public Use Dataset](https://data.cdc.gov/)

The full dataset has 100M+ records, which is impractical to process in a standard Colab environment. A stratified sample of 100,000 records was drawn and saved to Google Drive for reproducibility.

**Original attributes:**
`cdc_case_earliest_dt`, `cdc_report_dt`, `pos_spec_dt`, `current_status`, `sex`, `age_group`, `race_ethnicity_combined`, `hosp_yn`, `icu_yn`, `death_yn`, `medcond_yn`, `onset_dt`

## 🎯 Target Variable — `risk_level`

| Risk Level     | Value | Definition                                  |
|-----------------|:-----:|----------------------------------------------|
| 🟢 Low Risk     | 0     | No hospitalization                          |
| 🟡 Medium Risk  | 1     | Hospitalized, no ICU admission or death     |
| 🔴 High Risk    | 2     | ICU admission or death                      |

## 🧹 Data Preprocessing

- Loaded the 100,000-record sample
- Handled missing/unknown values
- Selected relevant features
- Derived the `risk_level` target
- Removed invalid/unusable records

**Result:** 100,000 sampled → **35,940 usable records**

## 🔑 Features

**Used as model inputs:**
`age_group`, `sex`, `medcond_yn`, `race_ethnicity_combined`

**Excluded from inputs (used only to build the target):**
`hosp_yn`, `icu_yn`, `death_yn` — including these as features would cause data leakage.

## 🧠 Models

Data was split 80% train / 20% test with a fixed random seed for reproducibility.

### 1️⃣ Rule-Based Classifier
Manually defined decision rules, e.g.:
- Age 70+ with a medical condition → 🔴 High Risk
- Age 50–69 with a medical condition → 🟡 Medium Risk
- Otherwise → 🟢 Low Risk

### 2️⃣ Bayesian Classifier
Implemented from scratch using Bayes' Theorem to compute `P(Risk | Features)` from:
`P(Risk)`, `P(Age | Risk)`, `P(Sex | Risk)`, `P(Medical Condition | Risk)`, `P(Race/Ethnicity | Risk)`

Log-probabilities were used for numerical stability.

## 📈 Performance

- **Bayesian Classifier Accuracy:** ~87.28% ✅
- The dataset is heavily imbalanced toward Low Risk cases, so accuracy alone isn't a complete measure of performance.
- Rule-Based and Bayesian classifiers are compared directly.

## 📊 Visualizations

- Model accuracy comparison
- Actual risk distribution
- Bayesian predicted risk distribution
- Actual vs. predicted risk levels

## 🧪 Manual & Interactive Testing

The system supports both hypothetical scenario testing and interactive user input (age group, sex, medical condition, race/ethnicity), returning predictions from both classifiers.

**Example:**
```
Age Group: 80+ Years
Sex: Male
Medical Condition: Yes
Race/Ethnicity: Missing

Rule-Based Prediction: 🔴 High Risk
Bayesian Prediction: 🔴 High Risk
```

## 🛠️ Tech Stack

- Python (Google Colab, Google Drive for data storage)
- Libraries: `numpy`, `pandas`, `matplotlib`, `seaborn`, `math`, `os`
- **🚫 Not used:** `scikit-learn`, `tensorflow`, `pytorch`, `xgboost`, `lightgbm`, `statsmodels` — all classification and probability logic was implemented manually.

## 📂 Project Structure

```
COVID_Risk_Classifier/
├── COVID_Risk_Classifier.ipynb
├── README.md
├── data/
│   ├── cdc_stratified_sample_100k.csv
│   └── cdc_cleaned_data.csv
└── results/
    ├── model_comparison_results.csv
    └── final_model_visualization.png
```

## 🔄 Workflow

```
CDC Dataset → Stratified Sampling (100k) → Preprocessing → Feature Engineering
→ Risk Level Creation → Train/Test Split → Rule-Based & Bayesian Classifiers
→ Performance Analysis → Visualization → Manual/Interactive Testing
```

## ⚠️ Limitations

- Imbalanced dataset
- Risk levels defined via a fixed rule, not clinically validated
- Rule-based model uses simplified decision logic
- Limited to selected demographic and medical features
- Accuracy alone doesn't fully capture performance on imbalanced classes

## 🚀 Future Improvements

- Manual implementation of Precision, Recall, and F1-Score
- Custom confusion matrix
- Better class imbalance handling
- Additional relevant features
- Testing on larger representative samples
- More classification algorithms implemented from scratch

## 📌 Conclusion

This project builds an end-to-end COVID-19 risk classification pipeline from real-world CDC surveillance data — processing a 100,000-record stratified sample into 35,940 usable records and comparing a Rule-Based Classifier against a Bayesian Classifier, both implemented manually in Python.

## 📚 Data Source

CDC COVID-19 Case Surveillance Public Use Dataset — [CDC Data Portal](https://data.cdc.gov/)

## 👨‍💻 Author

**Anirudh Patekar**
