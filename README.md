# Customer Churn Prediction & Risk Segmentation

A machine learning project focused on predicting customer churn and segmenting customers based on their probability of leaving a telecommunications service.

## 📌 Project Overview

Customer churn is a major business problem for subscription-based companies. Identifying customers who are likely to leave allows businesses to take preventive retention actions before the customer churns.

This project analyzes customer demographics, tenure, services, contract information, payment methods, and billing behavior to:

* Explore customer churn patterns
* Identify important churn drivers
* Build machine learning models to predict churn
* Compare multiple classification algorithms
* Tune the best-performing model
* Segment customers into Low, Medium, and High Risk groups
* Translate model outputs into actionable business recommendations

The project was completed as a **Week 2 Internship Project**.

---

## 📊 Dataset

The project uses the **Telco Customer Churn** dataset.

### Dataset Size

* **Rows:** 7,043
* **Columns:** 21
* **Target variable:** `Churn`

The dataset contains customer information including:

| Category             | Variables                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Customer Information | `customerID`, `gender`, `SeniorCitizen`, `Partner`, `Dependents`                                                         |
| Tenure               | `tenure`                                                                                                                 |
| Phone Services       | `PhoneService`, `MultipleLines`                                                                                          |
| Internet Services    | `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies` |
| Contract & Billing   | `Contract`, `PaperlessBilling`, `PaymentMethod`                                                                          |
| Charges              | `MonthlyCharges`, `TotalCharges`                                                                                         |
| Target               | `Churn`                                                                                                                  |

## The original dataset contains 5,174 customers who did not churn and 1,869 customers who churned, giving a churn rate of approximately **26.5%**.

## 🔍 Exploratory Data Analysis

The exploratory analysis includes:

* Dataset structure and data types
* Churn class distribution
* Missing-value analysis
* Numerical summary statistics
* Correlation analysis
* Churn patterns across customer characteristics

`TotalCharges` was converted from string to numeric format. This revealed **11 missing values**, which were handled during preprocessing.

Numerical variables analyzed include:

* `tenure`
* `MonthlyCharges`
* `TotalCharges`

---

## 🛠️ Data Preprocessing

The following preprocessing steps were performed:

1. Converted `TotalCharges` to numeric format.
2. Created additional features.
3. Encoded the target variable:

   * `No → 0`
   * `Yes → 1`
4. Removed `customerID` because it is an identifier rather than a predictive feature.
5. Applied median imputation to numerical variables.
6. Applied most-frequent imputation to categorical variables.
7. Applied `StandardScaler` to numerical features.
8. Applied one-hot encoding to categorical features.
9. Used a stratified 80/20 train-test split.

### Train/Test Split

* Training set: **5,634 records**
* Test set: **1,409 records**

The preprocessing pipeline resulted in **32 model-ready features**.

---

## ⚙️ Feature Engineering

Two additional features were created:

### 1. ChargesPerMonth

Calculated using:

```text
TotalCharges / tenure
```

For customers with zero tenure, the value was set to zero.

### 2. SeniorWithNoSupport

A binary feature identifying customers who are:

* Senior citizens
* Without Tech Support

These features were created to capture additional customer behavior and risk characteristics.

---

## 🤖 Machine Learning Models

Three classification algorithms were trained and compared:

1. **Logistic Regression**
2. **Random Forest**
3. **Gradient Boosting**

The evaluation metrics were:

* Accuracy
* Precision
* Recall
* F1-Score
* ROC-AUC

### Model Comparison

| Model               | Accuracy | Precision |     Recall |   F1-Score | ROC-AUC |
| ------------------- | -------: | --------: | ---------: | ---------: | ------: |
| Logistic Regression |   0.8070 |    0.6604 | **0.5615** | **0.6069** |  0.8421 |
| Random Forest       |   0.7885 |    0.6310 |     0.4893 |     0.5512 |  0.8255 |
| Gradient Boosting   |   0.7999 |    0.6586 |     0.5107 |     0.5753 |  0.8414 |

Logistic Regression initially provided the strongest F1-score and a strong ROC-AUC, while Gradient Boosting was subsequently tuned for final model selection.

---

## 🎯 Hyperparameter Tuning

Gradient Boosting was selected for further tuning using `GridSearchCV` with 3-fold cross-validation and ROC-AUC as the scoring metric.

### Parameter Grid

```text
n_estimators: [100, 200]
learning_rate: [0.05, 0.1]
max_depth: [3, 4]
```

### Best Parameters

```text
learning_rate = 0.05
max_depth = 3
n_estimators = 100
```

### Tuned Model Performance

* **ROC-AUC:** 0.8454
* **F1-Score:** 0.5645

The tuned Gradient Boosting model was selected as the final model because of its improved ROC-AUC and ability to provide feature importance information.

---

## 🚦 Customer Risk Segmentation

The final model's predicted churn probabilities were used to classify customers into three risk tiers.

| Churn Probability | Risk Tier   |
| ----------------- | ----------- |
| < 40%             | Low Risk    |
| 40% – 70%         | Medium Risk |
| ≥ 70%             | High Risk   |

### Test Set Risk Distribution

| Risk Tier   | Customers |
| ----------- | --------: |
| Low Risk    |     1,009 |
| Medium Risk |       311 |
| High Risk   |        89 |

The High Risk group represents customers with a predicted churn probability of at least 70%.

---

## 🔎 High-Risk Customer Profile

The analysis identified a clear profile among high-risk customers:

* Average tenure: approximately **3 months**
* Average monthly charges: approximately **$83**
* Almost exclusively **month-to-month contracts**

Medium-risk customers also showed relatively short tenure and higher monthly charges compared with the low-risk group.

The contract distribution showed that all **89 High Risk customers** were on month-to-month contracts in the analyzed test set.

---

## 📈 Visualizations

The project generates several visualizations to communicate the analysis:

### Feature Importance

Displays the most influential features in the tuned Gradient Boosting model.

### Churn Rate by Contract Type

Compares churn rates across:

* Month-to-month
* One year
* Two year

### Tenure Distribution

Compares tenure distributions between churned and non-churned customers.

### Customer Risk Distribution

A donut chart showing the proportion of customers in:

* Low Risk
* Medium Risk
* High Risk

---

## 💡 Key Business Insights

### 1. Short Tenure Is a Major Churn Risk

Customers with short tenure show substantially higher churn behavior. Customers who remain beyond the early stage of their relationship are considerably more likely to stay.

### 2. Month-to-Month Contracts Have Higher Churn

Customers on month-to-month contracts have substantially higher churn rates than customers with longer-term contracts.

The analysis reports approximately:

* **42% churn** for month-to-month contracts
* **Less than 10% churn** for two-year contracts

### 3. High Monthly Charges Increase Risk

Customers with higher monthly bills, particularly those without long-term commitments, show higher churn probability.

### 4. High-Risk Customers Are Mostly New Customers

The High Risk segment has an average tenure of approximately three months, indicating that the early customer lifecycle is a particularly important retention window.

---

## 💼 Business Recommendations

### Early-Tenure Retention Offer

Automatically provide a loyalty discount or free service upgrade, such as:

* Tech Support
* Online Security

to High Risk month-to-month customers after approximately 30 days.

### Contract Upgrade Incentive

Encourage month-to-month customers to move to one-year contracts within their first three months by offering a **10–15% discount**.

These recommendations target the two major risk factors identified in the analysis: **short tenure and month-to-month contracts**.

---

## ⚠️ Model Limitations

The project has several limitations:

* The dataset has a **73% / 27% class distribution**, resulting in moderate recall for churners.
* `TotalCharges` contains 11 missing values that were imputed using the median.
* The model does not incorporate temporal information such as usage or billing trends over time.
* External factors such as competitor pricing and network outages are not included.
* Recall for churned customers could potentially be improved using techniques such as SMOTE or class-weight adjustments.

These limitations should be considered before deploying the model in a production environment.

---

## 🧰 Technologies Used

### Programming Language

* Python

### Libraries

* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn

### Machine Learning

* Logistic Regression
* Random Forest Classifier
* Gradient Boosting Classifier
* GridSearchCV

### Evaluation

* Accuracy
* Precision
* Recall
* F1-Score
* ROC-AUC
* Confusion Matrix
* ROC Curve

---

## 📁 Project Structure

```text
Customer-Churn-Prediction/
│
├── analysis_fixed(4).ipynb
├── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── model_comparison.png
│
└── charts/
    ├── confusion_matrices.png
    ├── feature_importance.png
    ├── churn_by_contract.png
    ├── tenure_kde.png
    └── risk_tier_donut.png
```

> Update the filenames in this section if you rename the notebook or dataset before uploading the project to GitHub.

---

## ▶️ How to Run the Project

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd Customer-Churn-Prediction
```

### 2. Install the required libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 3. Launch Jupyter Notebook

```bash
jupyter notebook
```

### 4. Open the notebook

Open:

```text
analysis_fixed(4).ipynb
```

Make sure the dataset is located in the same directory as the notebook:

```text
WA_Fn-UseC_-Telco-Customer-Churn.csv
```

### 5. Run the cells

Execute the notebook from top to bottom to reproduce the analysis, model training, evaluation, risk segmentation, and visualizations.

---

## 📌 Project Outcome

This project demonstrates an end-to-end customer churn analytics workflow:

```text
Raw Customer Data
        ↓
Exploratory Data Analysis
        ↓
Data Cleaning & Preprocessing
        ↓
Feature Engineering
        ↓
Train/Test Split
        ↓
Model Training
        ↓
Model Comparison
        ↓
Hyperparameter Tuning
        ↓
Churn Probability Prediction
        ↓
Customer Risk Segmentation
        ↓
Business Insights
        ↓
Retention Recommendations
```

The final tuned Gradient Boosting model achieved a **ROC-AUC of 0.8454**, providing a useful basis for identifying customers who may require targeted retention interventions.

---

## 👤 Author

**Devanshu Chakravarty**

B.Tech Undergraduate Student

---

## ⭐ Project Focus

**Machine Learning | Customer Analytics | Churn Prediction | Risk Segmentation | Business Intelligence**
