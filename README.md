[![conda package](https://github.com/Valeriisht/eRNAi_project/actions/workflows/conda.yml/badge.svg)](https://github.com/Valeriisht/eRNAi_project/actions/workflows/conda.yaml?label=build)

Public Health Hackathon 2025 

<img align=right src="https://github.com/user-attachments/assets/81a66588-fca4-45ba-a0fa-e89731878b03" alt="# Early Detection of Blood Disorders Predictive Modeling from Lab and Outpatient Data" width="100"/>

The task involved developing a model capable of predicting hematological diagnoses at early stages using patients’ laboratory test results. Data were obtained from 38,041 patients of the Federal State Budgetary Institution "National Medical Research Centre of Hematology" under the Ministry of Health of the Russian Federation, including complete blood count (CBC) and biochemical blood test results.

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

# **Data Preprocessing**

During initial data analysis, hematological and oncohematological diseases were selected; all other diagnoses were grouped into an "Other" category. Diseases were further grouped based on clinical similarity according to the ICD-10 classification. Group selection was also guided by the number of individuals represented with each condition. The following disease groups were selected: 'D69', 'D47', 'C92', 'C91', 'C90', 'C83', 'D46', 'D50', 'D70', 'D75', 'C81', 'C85', 'C82', 'D45', 'D59', 'D72', 'D61'.

It was assumed that during a patient’s first clinical visit, no treatment had yet been administered; thus, these data are suitable for early diagnosis modeling. The target variable for prediction was the corresponding clinical diagnosis. Therefore, only the earliest recorded visit per patient was selected.

CBC and biochemical test results were selected according to the following criteria:
- If the earliest dates for both test types coincided, both results were included in the patient’s record.  
- If the dates differed by less than one day, both results were also included.  
- If the difference between the earliest CBC and biochemical test dates exceeded one day, it was assumed that the patient might have already started treatment; therefore, only the earlier of the two tests was retained.

# Models

## Prediction of Disease Groups

## Prediction of Specific Diseases

The top 50 most prevalent diseases among patients were selected for analysis.

An experiment was conducted using the following models: LogisticRegression (One-vs-Rest), multinomial LogisticRegression, and RandomForest.

Subsequently, a CatBoostClassifier was trained on each subset of 6 diseases from the selected 50. Performance metrics were computed and compared, and confusion matrices were examined. Diseases that demonstrated the clearest separation were selected. The final model is presented for this subset of diseases.

Differential diagnosis of hematological disorders is challenging due to the similarity of their clinical presentations. Our model enables highly accurate prediction of specific pathologies, aiding in the identification of patients requiring urgent care and narrowing the differential diagnostic workup.
