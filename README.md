# Clinical-Hybrid-Measurement-BreastCancer
A clinical-potential hybrid digital measurement framework for interpretable breast cancer malignancy quantification using Stacking Ensemble (LR, RF, SVM, XGBoost) and SHAP explainability. Achieves 98.6% Accuracy, 0.998 AUC, and 0.017 ECE.

# 🧬 Clinical-Hybrid-Measurement-BreastCancer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![scikit-learn](https://img.shields.io/badge/scikit-learn-yellow.svg)](https://scikit-learn.org/stable/install.html)
[![DOI](https://img.shields.io/badge/DOI-10.101016/100659-blue)](https://shorturl.at/NveLh)

## Beyond Classification: A Digital Measurement Instrument for Oncology

Traditional machine learning models in medical imaging are often criticized for being "black boxes"—they provide a result without explaining the *why*. 

**This repository introduces a paradigm-shifting hybrid digital measurement system** that transforms breast cancer diagnosis from subjective interpretation into a precise, interpretable, and metrologically-sound quantitative science. This is **not just a classifier**; it is a **Digital Measurement Instrument** for precision oncology, built to bridge the gap between high-performance AI and reliable, trustworthy clinical decision-making.

---

## The Innovation: Why This is a Scientific Breakthrough

### 1. From "Black-Box" to "White-Box" Clinical Trust
Unlike conventional models that output a binary label, our framework functions as a true **Metrological Device**. By leveraging **SHAP (SHapley Additive exPlanations)** game-theoretic interpretability, it doesn't just say *"Malignant"*; it provides a transparent, additive breakdown of **how each morphological feature** (e.g., nuclear contour, perimeter size) contributed to that decision. This allows pathologists to validate AI predictions against established cytopathological knowledge, fundamentally reducing diagnostic uncertainty.

### 2. A Leakage-Proof Hybrid Stacking Architecture
We fuse four heterogeneous base learners—Logistic Regression (LR), Random Forest (RF), Support Vector Machine (SVM), and XGBoost—using a **leakage-proof stacking ensemble** with a Logistic Regression meta-learner. Using **5-fold out-of-fold predictions** prevents overfitting and ensures that the meta-learner sees only "unseen" data, guaranteeing metrological validity.

### 3. Clinical Reliability over Mere Accuracy
While achieving **98.6% Accuracy** and **0.998 ROC-AUC**, this framework prioritizes **calibration**. An **Expected Calibration Error (ECE) of 0.017** ensures that the predicted probabilities accurately reflect true malignancy likelihood—critical for clinical risk stratification. 

> **Key Clinical Impact:** Achieved a **50% reduction in false-negative malignant cases** (from 2 to 1) compared to the best individual base model, directly addressing the most critical failure mode in cancer screening.

---

## Methodology & Architecture

### The Measurement Framework
The system ingests nine highly discriminative morphological features extracted from digitized FNA (Fine-Needle Aspiration) cytology images:

1. **concave points_worst** (Contour irregularity)
2. **perimeter_worst** (Size of most abnormal nucleus)
3. **radius_worst** (Mean radius of most abnormal nucleus)
4. **area_worst** (Area of most abnormal nucleus)
5. **concavity_mean** (Severity of contour concavity)
6. **radius_mean** (Mean nuclear radius)
7. **perimeter_mean** (Mean nuclear perimeter)
8. **area_mean** (Mean nuclear area)
9. **concavity_worst** (Worst concavity severity)

### Ensemble Architecture
The StackingClassifier integrates four diverse "measurement instruments" with a Meta-Learner:

| Instrument (Base Learner) | Metrological Role |
| :--- | :--- |
| **Logistic Regression (LR)** | Reference linear probabilistic instrument with high calibration |
| **Random Forest (RF)** | Non-linear pattern detector, robust to noise and outliers |
| **Support Vector Machine (SVM)** | High-dimensional geometric separator |
| **XGBoost** | Complex interaction capture with gradient boosting |
| **Meta-Learner (LR)** | Learns the optimal weighted combination of the instrument readings to produce the final calibrated risk estimate. |

---

## 📊 Performance Metrics (Validation on Hold-out Test Set)

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.986 |
| **Precision (Malignant)** | 0.981 |
| **Recall (Malignant)** | 0.981 |
| **F1-Score (Malignant)** | 0.981 |
| **ROC-AUC** | 0.998 |
| **Brier Score** | 0.042 |
| **Expected Calibration Error (ECE)** | **0.017** |

> **Calibration Curve:** The reliability diagram shows near-perfect alignment with the diagonal, confirming that a 90% predicted probability truly corresponds to a 90% chance of malignancy.

---

## 🔬 Interpretability in Action (SHAP Analysis)

Interpretability is the cornerstone of this framework. SHAP provides exact additive contribution values for each of the nine input measurements.

### Global Feature Importance
- **concave points_worst** (Mean SHAP: 0.421)
- **perimeter_worst** (Mean SHAP: 0.384)
- **radius_worst** (Mean SHAP: 0.351)

These findings align precisely with cytopathological knowledge, where nuclear contour irregularity and size enlargement are hallmark features of malignancy.

### Local Explanations (Patient-Specific)
For each prediction, the framework generates:
- **Waterfall Plots**: Shows how each feature incrementally pushes the final logit score from a baseline to the final prediction.
- **Beeswarm Summaries**: Provides a global view of feature impacts across all patients.
- **Feature Contribution Tables**: Quantifies the "Impact on Prediction" for each feature of the specific patient.

---

## Understanding the Code: A "What-If" Guide to Parameters

This section explains the critical hyperparameters and why they are set as they are, empowering you to modify and experiment safely.

### 1. Data Preprocessing: The `StandardScaler`
- **What it does:** Uses `z-score` normalization.
- **Why StandardScaler?** We chose `StandardScaler` over `MinMaxScaler` or `RobustScaler` because it preserves the original distribution shape and relative uncertainty structure of the biological measurements. This is essential for SHAP contribution analysis to remain consistent and clinically interpretable. A `RobustScaler` would reduce the impact of outliers but at the cost of masking critical pathological signals.

### 2. The Base Learners
- **Random Forest (`n_estimators=300`)**: 
  - **What if you change it?** Reducing it to `100` increases variance and the risk of overfitting. Increasing it to `500` may yield marginal gains in bias reduction at a significant computational cost.
- **XGBoost (`eval_metric="logloss"`, `random_state=42`)**: 
  - **What if you add `scale_pos_weight`?** Given the moderate class imbalance (1:1.68), using `class_weight="balanced"` in LR and RF is sufficient. Adding `scale_pos_weight` in XGBoost could further penalize false negatives but might distort probability calibration.

### 3. Stacking Strategy (`cv=5`)
- **What it does:** Uses 5-fold stratified cross-validation to generate out-of-fold predictions.
- **What if you change it to `cv=3`?** A smaller fold number reduces computational cost but increases the risk of **information leakage** and meta-overfitting, as the base learners will have seen more of the data during training for the meta-features.
- **What if you set `passthrough=True`?** This would pass the original features to the meta-learner. We deliberately set it to `False` to let the meta-learner focus solely on the base-learners' probabilistic outputs, reducing complexity and preventing overfitting.

### 4. Meta-Learner (Logistic Regression)
- **Why Logistic Regression?** It provides an interpretable linear combination of the base learners' outputs. The coefficients of this meta-learner essentially act as "weights" for each instrument, revealing which base learner the ensemble trusts the most for different scenarios.

---

## Getting Started

### Prerequisites
- Python 3.10 or higher
- Jupyter Notebook (or VS Code with Python extension)

### Installation
1.  Clone the repository:
    ```bash
    git clone https://github.com/Sepehryi1/Clinical-Hybrid-Measurement-BreastCancer.git
    cd Clinical-Hybrid-Measurement-BreastCancer

Running the Framework
Launch Jupyter Notebook or open the folder in VS Code.

Open the notebook breast_cancer_V3.ipynb.

Run the cells sequentially. The final cell is an interactive "Patient Diagnosis" module.

Interactive Input: Enter the patient's morphological feature values when prompted. The system will output:

The final diagnosis (Benign/Malignant) with Confidence.

The strongest contributing biomarker for that specific patient.

SHAP Waterfall plot for visual interpretability.




## 📚 How to Cite / Attribution

If you use this code, framework, or methodology in your research or projects, please provide attribution by citing the associated research paper and linking back to this repository:

> Alireza Yahyaei, Mohammad Arabian, *"A robust, clinical-potential hybrid digital measurement framework for precise and interpretable quantification of breast cancer malignancy: Fusion stacking ensemble with SHAP explainability and visual uncertainty reduction"*, 2026. [https://shorturl.at/NveLh]
>
> **GitHub Repository:** [(https://github.com/Sepehryi1/Clinical-Hybrid-Measurement-BreastCancer)]

**License:** This project is distributed under the MIT License, which permits free use, modification, and distribution, provided proper attribution is given to the authors.

Made with ❤️ for a future of interpretable and trustworthy AI in healthcare.
sepehr yahhyaei
