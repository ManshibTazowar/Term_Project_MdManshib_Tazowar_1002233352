## Data-Driven Prediction of Unknown Foundation Bridge Depth Using Nondestructive Testing and Machine Learning


This repository contains a machine learning framework developed to evaluate and predict bridge foundation parameters using Nondestructive Testing (NDT) data. The primary goal is to mitigate streambed scour risk—the leading cause of bridge failure in the United States—by predicting foundation types and depths for bridges with missing structural records.

## Project Overview
* **Author:** Md Manshib Tazowar
* **Institution:** The University of Texas at Arlington
* **UTA ID:** 1002233352
* **Dataset:** 71 bridges in the TxDOT Fort Worth District.

## Abstract

 There are over 643,000 bridges across the United States where 65,000 were classified as having unknown foundations. Unknown foundation bridges pose critical challenges to Departments of Transportation (DOTs) particularly for assessing scour vulnerability and planning maintenance or retrofits. Traditional methods such as excavation or boring are costly, invasive, and impractical for broad deployment, highlighting the need for efficient data-driven alternatives. This study implements a machine learning (ML) framework to classify and predict unknown bridge foundation depth using field data obtained from Sonic Echo Impulse Response (SEIR), which is a non-destructive testing method. Structural and site-specific parameters such as age, geometry, soil conditions, and hydraulic environment are combined with SEIR test data to develop predictive models. Multiple supervised algorithms, including Random Forest, Support Vector Machine (SVM), logistic regression, and LightGBM are trained and validated using known-foundation bridge data. Model accuracy is benchmarked using different statistical matrices. The resulting model provided a scalable and interpretable approach to estimate foundation depth, improving infrastructure risk assessment and supporting data-informed asset-management decisions.


 ## Introduction

In Texas, more than 9,000 bridges have unknown foundations, 85% of which are located on local roads [1]. These bridges, often referred to as "off system" bridges, were typically designed and constructed by local government entities or private companies. A significant portion of these unknown foundation bridges, as recorded in the National Bridge Inventory (NBI) has made monitoring safety very challenging. Particularly the scour-critical ones caused by the flowing water and erodible soil conditions. Evaluating unknown foundations is also essential when planning bridge improvements. 

Traditional methods such as excavation, coring, and boring while effective are often costly, invasive, and impractical for widespread application in identifying unknown foundation characteristics. In response, a variety of surface and borehole-based nondestructive testing (NDT) techniques offer a less disruptive alternative due to their non-invasive, cost-effective, and field-deployable nature. 

Various Stress-wave methods such as Sonic Echo (SE), Impulse Response (IR), Ultra-Seismic, bending wave analysis, and borehole-based methods including PS, borehole sonic, and Crosshole Sonic Logging (CSL) are widely applied to assess foundation depths. Electro-magnetic approaches like electrical resistivity imaging (ERI), induced polarization, electromagnetic induction, and field modal vibration also offer valuable subsurface insights in certain conditions. However, the applicability of these methods can be constrained by subsurface conditions, accessibility, or foundation type.

Bridge foundations are generally categorized into two types: Shallow Foundations (e.g., Spread Footings), and Deep Foundations (e.g., Drilled Shafts, Piles), which extend deep into the soil and are generally resilient to scour. Without knowing if a foundation is deep or shallow, engineers cannot accurately calculate its failure risk. Furthermore, estimating the exact depth is crucial for calculating load capacity. Current methods to determine foundation depth, such as excavation or boreholes, are expensive and disruptive to traffic. Figure 1 shows the location of the 71 bridges that were used for determining this project to classify and predict depth of the foundation. The aim of this project is to solve this problem using data mining techniques. The primary objectives are defined as follows:

1. **Classification (Phase I):** Distinction between Deep and Shallow foundations is sought to prioritize high-risk bridges.

2. **Prediction (Phase II):** The continuous Depth (ft) of the foundation is estimated for load rating analysis using regression models.


## Methodology

1. **Data Preprocessing:** Imputed missing values (mitigating outlier skew in the $n=71$ sample), transformed 'Year Built' to 'Bridge Age', and applied Z-Score scaling to geometric features.

2. **Feature Selection:** Ranked critical structural drivers using:
   * **Pearson Correlation:** Captured direct linear relationships.
   * **Random Forest Importance:** Captured non-linear variable interactions.
   * **PLS-VIP Scores:** Handled multicollinearity and validated dominant drivers.

3. **Classification (Deep vs. Shallow):** Evaluated models using Stratified K-Fold Cross-Validation. LightGBM (98.6%) and Logistic Regression (97.1%) successfully classified foundation logic, primarily driven by *Pier Shape*.

4. **Depth Estimation (Pier Total Depth):** Evaluated regressors using GridSearchCV. The Random Forest Regressor predicted exact pier embedment depth ($R^2 = 0.7845$, RMSE = 10.44 ft), primarily driven by *Pier Unbraced Length* and *Max Span Length*.

5. **Robustness Stress-Testing:** Tested the champion models against data scarcity, missing values, noise perturbations, and rigorous bootstrap resampling (20 iterations) to ensure stability.

## Repository structure

```text
sample-github-repo/
├── data/
│   ├── raw_data/
│   │   ├── raw_classification_dataset.csv
│   │   ├── raw_prediction_dataset.csv
│   ├── processed_data/
│   │   ├── processed_classification_dataset.csv
│   │   ├── processed_prediction_dataset.csv
│   └── data_README.md
├── doc/
│   ├── CE 5316 Project Proposal_Md Manshib Tazowar_1002233352.docx
│   ├── CE 5316 Project Proposal_Md Manshib Tazowar_1002233352.pdf
│   ├── repository_checklist.md
│   └── suggested_workflow.md
├── output/
│   ├── accuracy/
│   │   ├── figures/
│   │   │   ├── classification_figures/
│   │   │   ├── prediction_figures/
│   │   ├── Classification_accuracy/
│   │   ├── Prediction_accuracy/
│   └── robustness/
│       └── Classification_robustness/
│       └── Prediction_robustness/
├── src/
│   ├── presentation_plots.ipynb
│   ├── all_plots.ipynb
│   ├── model.ipynb
│   ├── plot_accuracy_features.ipynb
│   ├── process_data.ipynb
│   └── robustness_checks.ipynb
├── LICENSE
├── README.md
```

## Expected Outputs

Running the pipeline generates the following files in the `output/` directory:

### Accuracy & Feature Selection
* `output/accuracy/class_model_comparison_metrics.csv`
* `output/accuracy/pred_model_comparison_metrics.csv`
* `output/accuracy/figures/class_confusion_matrix.png`
* `output/accuracy/figures/pred_metrics_comparison_dual_axis.png`
* `output/accuracy/figures/correlation_heatmap_lower_triangle.png`
* `output/accuracy/figures/feature_importance_xgboost_style.png`

### Robustness Checks
* `output/robustness/class_robustness_all_runs.csv`
* `output/robustness/pred_robustness_all_runs.csv`
* `output/robustness/figures/*_robustness_data_size.png`
* `output/robustness/figures/*_robustness_missing_data.png`
* `output/robustness/figures/*_robustness_perturbation.png`
* `output/robustness/figures/*_robustness_bootstrap_histogram.png`

## Conclusion

It was successfully demonstrated in this study that unknown bridge foundations can be characterized with high accuracy using only non-destructive testing and inventory data.

**Classification:** 98% accuracy was achieved in distinguishing Deep vs. Shallow foundations using LightGBM.

**Prediction:** An R2 of 0.78 was achieved in predicting specific foundation depths using Random Forest, with a mean error of 8.08 feet.
A scalable, data-driven tool is provided by this framework for DOTs to prioritize bridges for scouring countermeasures without the need for expensive excavation.


## Future Works

* **Data Expansion:** Increase the SEIR dataset sample size ($n > 200$) across diverse geographic locations to stabilize advanced boosting algorithms.
* **Geotechnical Map Integration:** Transition "Soil Type" to quantitative parameters (e.g., shear strength, bearing capacity) via GIS integration.
* **Hybrid Physics-Informed ML:** Incorporate physical constraints (e.g., minimum embedment length based on span load) as loss functions to guarantee physically realistic predictions.
* **Temporal Analysis:** Integrate time-series condition data to analyze degradation trends.
* **Explainable AI (XAI):** Utilize SHAP or LIME to ensure transparent, actionable insights for DOT stakeholders.


## Data Usage and Licensing Disclaimer

The datasets, models, and findings provided in this repository are derived from ongoing, active research conducted by The University of Texas at Arlington (UTA) with the Texas Department of Transportation (TxDOT) Fort Worth District.

Because bridge foundation records and structural geometry represent critical national infrastructure assets, strict usage limitations apply to this project:

• **No Unauthorized Copying or Distribution:** You may not copy, share, publish, or distribute the raw or processed datasets contained within this project without explicit written permission from the primary researchers and TxDOT.

• **Restricted Use:** The data is provided strictly for academic review and demonstration of the machine learning workflow within the context of this specific project.

• **Prohibition of Misuse:** Do not misuse, reverse-engineer, or apply this data to assess or make decisions regarding physical, real-world infrastructure.

For permissions or inquiries regarding the dataset, please contact the repository owner or the UTA Civil Engineering research team.
"""