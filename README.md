# Blocker-Fraud-Detection

## Overview
Blocker Fraud is a performance-based service that detects and blocks fraudulent mobile transactions. In Brazil, the company charges:

- **25%** of each truly fraudulent transaction it catches
- **5%** of each legitimate transaction it wrongly flags as fraud
- **100% refund** to clients for undetected fraud

This aggressive pricing drives rapid customer acquisition but places financial risk on the model’s accuracy.

## Business Context
- Fraud losses in Brazil can exceed R$ 1 billion annually, driving banks and fintechs to invest heavily in anti-fraud systems.
- Blocker Fraud’s revenue scales with its model’s precision—errors can lead to substantial losses.

## Solution Approach
We built a machine-learning pipeline to predict fraud and measured its revenue impact:

1. **Data Ingestion & Cleaning**  
   - Load raw transactions  
   - Remove irrelevant fields and handle missing values
2. **Feature Engineering**  
   - Create new features (e.g., hour of day, log amount) based on domain hypotheses
3. **Exploratory Data Analysis**  
   - Univariate/bivariate analysis  
   - Hypothesis testing (e.g., transaction type distributions)
4. **Data Preparation**  
   - Encode categorical variables  
   - Scale numeric features  
   - Address class imbalance with SMOTE or undersampling
5. **Feature Selection**  
   - Use Boruta to pick the most predictive features
6. **Model Training & Validation**  
   - Compare Dummy, LR, KNN, SVM, Random Forest, XGBoost, LightGBM  
   - 5-fold cross-validation to evaluate balanced accuracy, precision, recall, F1, Kappa
7. **Hyperparameter Tuning**  
   - Optimize XGBoost with grid/random search
8. **Final Evaluation**  
   - Test on unseen data to simulate live performance
9. **Deployment**  
   - Serve the trained model via a Flask API

## Key Data Insights
- **Fraud amounts** are all above R$ 10 000, but non-fraud can exceed R$ 100 000.
- Fraud splits nearly evenly between **transfer** and **cash-out** types.
- Transactions > R$ 100 000 occur across **transfer**, **cash-out**, and **cash-in**.

## Model Comparison
| Model               | Bal. Acc. | Precision | Recall | F1    | Kappa |
|---------------------|-----------|-----------|--------|-------|-------|
| Dummy               | 0.499     | 0.000     | 0.000  | 0.000 | -0.001|
| Logistic Regression | 0.565     | 1.000     | 0.129  | 0.229 | 0.228 |
| K Nearest Neighbors | 0.705     | 0.942     | 0.409  | 0.568 | 0.567 |
| SVM                 | 0.595     | 1.000     | 0.190  | 0.319 | 0.319 |
| Random Forest       | 0.865     | 0.972     | 0.731  | 0.834 | 0.833 |
| **XGBoost**         | **0.880** | **0.963** | **0.761** | **0.850** | **0.850** |
| LightGBM            | 0.701     | 0.180     | 0.407  | 0.241 | 0.239 |

After tuning, **XGBoost** achieved:
- **Balanced Accuracy:** 0.915  
- **Precision:** 0.944  
- **Recall:** 0.829  
- **F1 Score:** 0.883

## Business Impact
- **True fraud detections (25% fee):** R$ 60,613,782.88  
- **False positives (5% fee):** R$ 183,866.98  
- **Missed fraud refunds (100% loss):** R$ 3,546,075.42

**Net profit:** R$ 57,251,574.44 (vs. –R$ 246,001,206.94 without the model)

## Conclusions
- High-class imbalance can still yield robust models with proper sampling and features.
- Rare fraud cases (<1%) are learnable.

## Next Steps
- Test additional business hypotheses in EDA  
- Experiment with more sampling techniques (e.g., ADASYN, Tomek links)  
- Deploy the Flask API on Heroku for live testing

