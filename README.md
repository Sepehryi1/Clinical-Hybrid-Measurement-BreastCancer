# Clinical-Hybrid-Measurement-BreastCancer
A clinical-potential hybrid digital measurement framework for interpretable breast cancer malignancy quantification using Stacking Ensemble (LR, RF, SVM, XGBoost) and SHAP explainability. Achieves 98.6% Accuracy, 0.998 AUC, and 0.017 ECE.
Revolutionizing Oncology: From Black-Box to Clinical Trust
Traditional machine learning models in medical imaging are often criticized for being "black boxes"—they provide a result without explaining the why. This repository introduces a paradigm-shifting hybrid digital measurement system that transforms breast cancer diagnosis from subjective interpretation into a precise, interpretable, and metrologically-sound quantitative science.

By fusing four heterogeneous base learners (Logistic Regression, Random Forest, Support Vector Machine, and XGBoost) through a leakage-proof stacking ensemble with a Logistic Regression meta-learner, and coupling it with SHAP (SHapley Additive exPlanations) game-theoretic interpretability, this framework does more than just classify—it quantifies malignancy risk, identifies the strongest contributing biomarkers for each patient, and reduces diagnostic uncertainty.

Key Breakthroughs:

Exceptional Performance: 98.6% Accuracy, ROC-AUC of 0.998, and a Brier Score of 0.042 on the Wisconsin Breast Cancer dataset.

Clinical Reliability: An Expected Calibration Error (ECE) of 0.017 ensures that the predicted probabilities accurately reflect true malignancy likelihood—critical for clinical risk stratification.

Interpretability by Design: Leveraging SHAP, the system provides intuitive waterfall plots, beeswarm summaries, and feature dependence plots, allowing pathologists to validate predictions against established cytopathological knowledge (e.g., concave points_worst, perimeter_worst).

Reducing Critical Errors: A 50% reduction in false-negative malignant cases (from 2 to 1) compared to the best individual base model, directly addressing the most critical failure mode in cancer screening.

This is not just a classification script; it is a Digital Measurement Instrument for precision oncology, built to bridge the gap between high-performance AI and reliable, trustworthy clinical decision-making.

## 📚 How to Cite / Attribution

If you use this code, framework, or methodology in your research or projects, please provide attribution by citing the associated research paper and linking back to this repository:

> Alireza Yahyaei, Mohammad Arabian, *"A robust, clinical-potential hybrid digital measurement framework for precise and interpretable quantification of breast cancer malignancy: Fusion stacking ensemble with SHAP explainability and visual uncertainty reduction"*, 2026. [https://shorturl.at/NveLh]
>
> **GitHub Repository:** [(https://github.com/Sepehryi1/Clinical-Hybrid-Measurement-BreastCancer)]

**License:** This project is distributed under the MIT License, which permits free use, modification, and distribution, provided proper attribution is given to the authors.
