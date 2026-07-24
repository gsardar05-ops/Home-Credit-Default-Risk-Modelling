# Home Credit Default Risk Modelling

An end-to-end credit-risk modelling project using the **Home Credit Default Risk** dataset to estimate the likelihood of repayment difficulty among loan applicants.

The project combines exploratory data analysis, feature engineering, Probability of Default modelling, model validation, borrower risk segmentation and Expected Loss analysis. The objective is not only to build a predictive model, but also to translate its outputs into practical credit-risk insights.

---

## Business Problem

Financial institutions must identify applicants who are more likely to experience repayment difficulty while avoiding the unnecessary rejection of creditworthy customers.

This creates an important trade-off:

- Approving a high-risk applicant can increase future credit losses.
- Rejecting a creditworthy applicant can result in lost business and poor customer experience.

This project develops a structured credit-risk framework to support more informed lending decisions using applicant-level financial, demographic and credit-related information.

---

## Project Objectives

- Understand the structure and quality of the Home Credit application data.
- Analyse repayment difficulty and target-class imbalance.
- Identify important applicant and affordability-related risk variables.
- Develop an interpretable baseline Probability of Default model.
- Engineer additional features to improve model performance.
- Validate the model using appropriate classification and ranking metrics.
- Evaluate alternative decision thresholds.
- Segment applicants into meaningful risk categories.
- Demonstrate Expected Loss estimation using PD, LGD and EAD.

---

## Project Status

- [x] Repository structure and environment setup
- [x] Data understanding
- [x] Exploratory data analysis
- [x] Missing-value and feature-type analysis
- [x] Baseline Probability of Default model
- [x] Feature engineering
- [x] Model validation
- [x] Borrower risk segmentation
- [x] Expected Loss framework
- [x] Final project documentation

---

## Methodology

The project follows the workflow below:

1. **Data understanding**  
   Reviewed dataset dimensions, target definition, feature types and data quality.

2. **Exploratory data analysis**  
   Analysed repayment difficulty, class imbalance, applicant characteristics and selected risk variables.

3. **Missing-value and feature analysis**  
   Assessed missingness, numerical and categorical variables, and potential modelling challenges.

4. **Baseline Probability of Default model**  
   Developed an interpretable baseline model to establish a performance benchmark.

5. **Feature engineering**  
   Created additional applicant, affordability and credit-related variables.

6. **Model validation**  
   Evaluated predictive performance, ranking ability, classification outcomes and threshold trade-offs.

7. **Expected Loss framework**  
   Combined model-generated default-risk scores with an assumed LGD and `AMT_CREDIT` as an EAD proxy to demonstrate Expected Loss analysis on the validation sample.

---

## Notebook Guide

| Notebook | Description |
|---|---|
| `01_data_understanding.ipynb` | Dataset structure, target variable, feature types and initial data-quality assessment |
| `02_application_train_eda.ipynb` | Exploratory analysis of applicants, repayment difficulty and class imbalance |
| `03_missing_values_and_feature_types.ipynb` | Missing-value patterns and numerical/categorical feature analysis |
| `04_baseline_pd_model.ipynb` | Baseline Probability of Default model and initial evaluation |
| `05_feature_engineering_application_data.ipynb` | Creation and assessment of additional risk-related variables |
| `06_model_validation.ipynb` | Model performance, threshold analysis and validation |
| `07_expected_loss_framework.ipynb` | Expected Loss estimation using PD, LGD and EAD |

---

## Dataset

This project uses the **Home Credit Default Risk** dataset available through Kaggle.

The target variable identifies applicants who experienced repayment difficulty.

For this educational project, `TARGET` is treated as a proxy for default risk or repayment difficulty. It should not be interpreted as a formal regulatory definition of default.

Raw data files are excluded from this repository because of their size. After downloading the dataset, place the required files inside:

```text
data/raw/
```

The primary file used in the initial modelling workflow is:

```text
application_train.csv
```

---

## Feature Engineering

The feature-engineering stage includes selected variables related to applicant affordability, credit exposure and financial capacity.

Implemented features include:

- Credit-to-income ratio
- Annuity-to-income ratio
- Credit-to-annuity ratio
- Applicant age
- Employment duration
- Employment-to-age ratio
- Income per family member
- Income per child
- Mean external credit score
- Document count

Only variables implemented in the project notebooks should be treated as part of the final model.

---

## Final Model

| Item | Result |
|---|---|
| Final model | Engineered Feature Logistic Regression Model |
| Training observations | 246,008 |
| Validation observations | 61,503 |
| Decision threshold | 0.50 |
| Post-encoding model features | 256 |

The engineered-feature logistic regression model was retained as the final model for this project because it marginally improved ROC-AUC over the baseline while preserving interpretability and incorporating business-relevant risk features.

Where possible, model interpretability was considered alongside predictive accuracy.

---

## Model Performance

| Metric | Validation Result |
|---|---:|
| ROC-AUC | 0.7492 |
| Precision — Default class | 0.1608 |
| Recall — Default class | 0.6747 |
| F1-score — Default class | 0.2598 |
| Accuracy | 0.6895 |
| Gini coefficient | 0.4983 |
| KS statistic | 0.3690 |

Because the target variable is imbalanced, model evaluation focuses primarily on:

- ROC-AUC
- Precision
- Recall
- F1-score
- Confusion matrix
- Threshold analysis

Accuracy is not treated as the primary measure of model quality.

---

## Threshold Analysis

The classification threshold determines how predicted probabilities are converted into default and non-default classifications.

A lower threshold may identify a larger proportion of risky applicants, but it can also incorrectly flag more creditworthy applicants. A higher threshold may reduce false positives but increase the number of risky applicants that are missed.

Alternative threshold choices were reviewed in relation to the trade-off between:

- Detecting applicants with repayment difficulty
- Limiting unnecessary rejection of creditworthy applicants
- The lender’s risk appetite
- Potential financial consequences of misclassification

**Selected threshold:** `0.50`

**Reason for selection:**  
The threshold of 0.50 was retained as a standard baseline decision threshold. It provides a consistent benchmark for evaluating classification performance but was not optimized for a specific lender objective. Future iterations can select the threshold using risk appetite, approval-rate targets, recall, precision and Expected Loss trade-offs.

---

## Borrower Risk Segmentation

Model-generated default-risk scores were converted into borrower risk categories to make the model outputs easier to interpret.

| Risk Segment | Model Score Range | Interpretation |
|---|---:|---|
| Low Risk | **Score < 5%** | Applicants with comparatively low predicted repayment risk |
| Medium Risk | **5% ≤ Score < 15%** | Applicants requiring standard monitoring and verification |
| High Risk | **15% ≤ Score < 30%** | Applicants requiring additional credit assessment |
| Very High Risk | **Score ≥ 30%** | Applicants with the highest predicted repayment risk |

These are illustrative rule-based risk bands designed to translate model scores into interpretable applicant segments. The boundaries are not empirically calibrated and would require validation before operational use.

---

## Expected Loss Framework

**Expected Loss = Probability of Default × Loss Given Default × Exposure at Default**

Or, equivalently:

`Expected Loss = PD × LGD × EAD`

Where:

- **Probability of Default:** Model-generated default-risk score used as a proxy for the likelihood of repayment difficulty
- **Loss Given Default:** Proportion of exposure expected to be lost if default occurs
- **Exposure at Default:** Estimated outstanding exposure when default occurs

### Implementation

- A model-generated default-risk score was used as a proxy for PD.
- LGD was assumed at **45% for demonstration**.
- EAD was approximated using `AMT_CREDIT`.
- Monetary outputs are reported in **dataset currency units**.

These assumptions are illustrative and should not be interpreted as independently validated production estimates.

### Expected Loss Output

| Measure | Result |
|---|---:|
| Validation-sample exposure | 36,765,080,145.00 dataset currency units |
| Average modelled default-risk score | 42.10% |
| Validation-sample expected loss | 6,715,201,851.40 dataset currency units |
| Expected loss rate | 18.27% |

These values are calculated on the validation sample and demonstrate the Expected Loss framework rather than represent a production-level portfolio loss estimate.

Because the model uses class weighting to address target imbalance, its probability outputs should primarily be interpreted as relative risk scores for ranking applicants. Probability calibration would be required before treating them as production-grade PD estimates.

---

## Key Findings

- The dataset displays significant class imbalance: 8.07% of applicants faced repayment difficulty, while 91.93% did not.
- `EXT_SOURCE_MEAN` was one of the strongest engineered risk indicators, with a correlation of -0.2221 with repayment difficulty.
- Engineered affordability variables such as credit-to-income ratio, annuity-to-income ratio and income per family member help explain applicant repayment pressure, although external score variables remained stronger predictors.
- Feature engineering slightly improved model performance relative to the baseline. Baseline ROC-AUC was 0.7483, while the engineered model ROC-AUC was 0.7492.
- Threshold selection materially affects the trade-off between identifying risky applicants and incorrectly flagging creditworthy applicants.
- Translating model-generated risk scores into borrower segments and an illustrative Expected Loss framework provides more actionable information than a default/non-default classification alone.

---

## Selected Visualisations

### Target-Class Distribution

![Target-Class Distribution](reports/figures/class_distribution.png)

The target distribution shows that repayment-difficulty cases form a relatively small proportion of the applicant dataset, confirming that the classification problem is imbalanced.

### ROC Curve

![ROC Curve](reports/figures/roc_curve.png)

The ROC curve illustrates the model’s ability to rank applicants with repayment difficulty above applicants without repayment difficulty across classification thresholds.

### Confusion Matrix

![Confusion Matrix](reports/figures/confusion_matrix.png)

The confusion matrix shows the number of correctly and incorrectly classified applicants at the selected decision threshold.

---

## Repository Structure

```text
Home-Credit-Default-Risk-Modelling/
├── dashboard/                 # Dashboard files and outputs
├── data/                      # Dataset instructions; raw files excluded
│   ├── raw/                   # Local raw dataset location
│   └── processed/             # Local processed outputs and model results
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_application_train_eda.ipynb
│   ├── 03_missing_values_and_feature_types.ipynb
│   ├── 04_baseline_pd_model.ipynb
│   ├── 05_feature_engineering_application_data.ipynb
│   ├── 06_model_validation.ipynb
│   └── 07_expected_loss_framework.ipynb
├── reports/
│   └── figures/               # EDA and model-evaluation visualisations
├── src/                       # Reusable Python scripts
├── .gitignore
├── README.md
└── requirements.txt
```

---

## How to Run

1. Clone the repository:

```bash
git clone https://github.com/gsardar05-ops/Home-Credit-Default-Risk-Modelling.git
```

2. Move into the project directory:

```bash
cd Home-Credit-Default-Risk-Modelling
```

3. Create and activate a Python virtual environment:

```bash
python -m venv venv
```

For Windows:

```bash
venv\Scripts\activate
```

For macOS/Linux:

```bash
source venv/bin/activate
```

4. Install the required libraries:

```bash
pip install -r requirements.txt
```

5. Download the Home Credit Default Risk dataset from Kaggle.

6. Place the required data files inside:

```text
data/raw/
```

The main file required for the initial modelling workflow is:

```text
application_train.csv
```

7. Run the notebooks in numerical order, beginning with:

```text
notebooks/01_data_understanding.ipynb
```

---

## Tools and Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter Notebook
- Git and GitHub

---

## Current Limitations

- The model is developed using historical competition data rather than live lending data.
- The analysis primarily uses applicant-level information available in the selected dataset.
- Class imbalance affects the interpretation of standard classification metrics.
- Model performance has not been tested on external institutional data.
- LGD and EAD may rely on simplifying assumptions where sufficient default and recovery data are unavailable.
- Model stability, fairness and regulatory suitability would require additional validation before real-world use.
- The project is intended for educational and portfolio purposes rather than direct lending decisions.

---

## Potential Improvements

- Include additional Home Credit relational datasets.
- Create aggregated features from previous applications and credit history.
- Compare additional tree-based and boosting models.
- Perform probability calibration and stability analysis.
- Conduct fairness and bias assessment across applicant groups.
- Develop independent LGD and EAD models where suitable data are available.
- Build an interactive credit-risk monitoring dashboard.
- Evaluate model performance using out-of-time validation.

---

## Author

**Gourav Manohar Sardar**

MBA candidate at IIM Bodh Gaya with interests in financial risk modelling, business analytics, data analysis and technology-driven decision-making.

[LinkedIn](https://www.linkedin.com/in/gourav-sardar)