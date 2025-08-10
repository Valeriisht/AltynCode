# Early Detection of Blood Disorders Predictive Modeling from Lab and Outpatient Data

<img align=right src="https://spaces-cdn.clipsafari.com/0sy12l8yao55bk3gzl4ad1ox6fs5" alt="# Early Detection of Blood Disorders Predictive Modeling from Lab and Outpatient Data" width="100"/>

[![conda package](https://github.com/Valeriisht/eRNAi_project/actions/workflows/conda.yml/badge.svg)](https://github.com/Valeriisht/eRNAi_project/actions/workflows/conda.yaml?label=build)

Public Health Hackathon 2025

# **Aim**

- The task involved developing a model capable of predicting hematological diagnoses at early stages using patients’ laboratory test results.

## Contents
- [About](#About)
- [Pipeline](#Pipeline)
- [Data Preprocessing](#Data-Preprocessing)
- [Methods](#Methods)
- [System requirements](#System-requirements)
- [Installation](#Installation)
- [Results](#Results)
- [References](#References)
- [Authors](#Authors)

# **About** 

Data were obtained from 38,041 patients of the Federal State Budgetary Institution "National Medical Research Centre of Hematology" under the Ministry of Health of the Russian Federation, including complete blood count (CBC) and biochemical blood test results.

It is expected that further application of this model in screening within primary healthcare settings will help raise clinical suspicion of hematological disorders, enabling timely patient referral for differential diagnosis.

Four datasets were provided:
- Пациенты (Patients)  
- Обращения (Visits)  
- ОАК (complete blood count)  
- Биохимия (biochemical blood analysis)

The data exhibited both strengths and limitations from the perspective of data analysis. Among the strengths was the fact that the data were real-world and conditionally labeled—i.e., for each patient (identified by unique patient IDs), information on confirmed diagnoses (within the hematology specialty) was available for every clinical visit.

However, several limitations were also identified:
1. No explicit "healthy" labels were provided.  
2. Limited data availability for certain diseases.  
3. Inconsistent sets of measured parameters across laboratory tests.  
4. The dataset includes test results obtained during ongoing treatment.

Additionally, technical data cleaning was performed (see details below).

# **Pipline** 


# **Data Preprocessing**

During initial data analysis, hematological and oncohematological diseases were selected; all other diagnoses were grouped into an "Other" category. Diseases were further grouped based on clinical similarity according to the ICD-10 classification. Group selection was also guided by the number of individuals represented with each condition. The following disease groups were selected: 'D69', 'D47', 'C92', 'C91', 'C90', 'C83', 'D46', 'D50', 'D70', 'D75', 'C81', 'C85', 'C82', 'D45', 'D59', 'D72', 'D61'.

It was assumed that during a patient’s first clinical visit, no treatment had yet been administered; thus, these data are suitable for early diagnosis modeling. The target variable for prediction was the corresponding clinical diagnosis. Therefore, only the earliest recorded visit per patient was selected.

CBC and biochemical test results were selected according to the following criteria:
- If the earliest dates for both test types coincided, both results were included in the patient’s record.  
- If the dates differed by less than one day, both results were also included.  
- If the difference between the earliest CBC and biochemical test dates exceeded one day, it was assumed that the patient might have already started treatment; therefore, only the earlier of the two tests was retained.


# **Methods**

The following tools are used in this project: 

### Core Frameworks & Libraries
- **Python 3.10+** as the primary programming language
- **Jupyter Notebook** for interactive analysis and documentation

### Machine Learning Ecosystem
- **Scikit-learn** (v1.2+) for traditional ML models:
  - Logistic Regression
  - Random Forest
  - Gradient Boosting
- **XGBoost/LightGBM/CatBoost** for advanced boosting algorithms
- **SHAP** (SHapley Additive exPlanations) for model interpretability

### Data Processing
- **Pandas** for data manipulation and analysis
- **NumPy** for numerical computations
- **SciPy** for statistical operations

### Visualization Tools
- **Matplotlib** & **Seaborn** for static visualizations

### Optimization & Deployment
- **Optuna** for hyperparameter tuning
- **MLflow** for experiment tracking (if applicable)
- **Pickle/Joblib** for model serialization

### Development Environment
- **VS Code** with Python extensions
- **Git** for version control

# System requirements

- The minimum requirements are as follows:

### Device requirements

- **Operation memory** - at least 4 GB
- **Processor** - x86-64 architecture,  4+ cores recommended

### Software requirements

- **Operation system**
  - Linux: Ubuntu 20.04+
  - Windows: 10/11 (64-bit)
  - macOS: 11.0+ (Big Sur)

### Recomended 

- SSD-disk
- 4+ core processor


# Installation

Install with:

```sh
git clone git@github.com:https://github.com/Valeriisht/AltynCode
conda env create -f enviromental.yaml
conda activate enviroment
```

# Results

## Models

## Prediction of Disease Groups

We trained machine-learning models on generalized diagnoses grouped by clinical and laboratory manifestations of diseases (ICD-10). Eighteen classes were identified.

```
According to the label mapping:
C81: 0
C82: 1
C83: 2
C85: 3
C90: 4
C91: 5
C92: 6
D45: 7
D46: 8
D47: 9
D50: 10
D59: 11
D61: 12
D69: 13
D70: 14
D72: 15
D75: 16
healthy: 17
other: 18
```

The main focus was on models with native data support (CatBoost, LightGBM, XGBoost) capable of effectively working with raw laboratory indicators, including missing values and categorical features.

For comparison, classical algorithms (Random Forest, logistic regression, GradientBoosting.) were also used after preliminary data processing.

### Model Selection Framework

#### Native Support Models (Handling Raw  Data)
- **CatBoost**
  - Automatic categorical feature handling
  - Built-in missing value support
  - Ordered boosting for medical data robustness

- **LightGBM**
  - Gradient-based One-Side Sampling (GOSS)
  - Exclusive Feature Bundling (EFB) for high-dimensional lab data

- **XGBoost**
  - Customizable missing value imputation
  - Strict regularization for medical applications

#### Complete Data Models (After Advanced Imputation)
- **Random Forest**
- **Gradient Boosting**
- **Logistic Regression**
- **KNN**

The optimal model was selected based on a combination of statistical metrics and clinical interpretability of the results, using SHAP analysis methods.

Validation was performed using stratified splitting and nested cross-validation.

## Prediction of Specific Diseases

The top 50 most prevalent diseases among patients were selected for analysis.

An experiment was conducted using the following models: LogisticRegression (One-vs-Rest), multinomial LogisticRegression, and RandomForest.

Subsequently, a CatBoostClassifier was trained on each subset of 6 diseases from the selected 50. Performance metrics were computed and compared, and confusion matrices were examined. Diseases that demonstrated the clearest separation were selected. The final model is presented for this subset of diseases.

Differential diagnosis of hematological disorders is challenging due to the similarity of their clinical presentations. Our model enables highly accurate prediction of specific pathologies, aiding in the identification of patients requiring urgent care and narrowing the differential diagnostic workup.

# References
1) Ning, W., Wang, Z., Gu, Y. et al. Machine learning models based on routine blood and biochemical test data for diagnosis of neurological diseases. Sci Rep 15, 27857 (2025). https://doi.org/10.1038/s41598-025-09439-4
2) Palak, Ishdeep Singla, Drishti Malhotra, Karan Kumar, "Machine Learning-Driven Insights into Autoimmune Disease Prediction and Patient Outcomes", 2024 International Conference on Cybernation and Computation (CYBERCOM), pp.466-470, 2024.

# Authors

- Stepan Epifantsev
- Valeria Ishtuganova
- Natalia Kondratiuk
- Anna Andreeva
- Dmitry Bessmertnyy
- Olya Piskunova 

