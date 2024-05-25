Refined Problem Statement:

Hospital readmissions within 30 days of discharge are a significant concern in healthcare due to their impact on patient well-being and healthcare costs. Our project aims to leverage machine learning to predict the likelihood of readmission within 30 days for patients with diabetes. By accurately identifying high-risk patients, we empower healthcare providers to implement targeted interventions, ultimately reducing readmissions, improving patient outcomes, and optimizing the allocation of hospital resources.

Business Value:

This project addresses a critical challenge in the healthcare industry, where reducing readmission rates is a key performance indicator. By deploying a predictive model, hospitals can:

Improve Patient Care: Identify patients who are at high risk of readmission and provide them with additional support, such as follow-up appointments, medication management, and education on self-care.
Reduce Costs: Avoid the financial burden associated with unplanned readmissions, which can be substantial for both patients and healthcare providers.
Optimize Resource Allocation: Focus resources on patients who need them most, ensuring that those at higher risk of readmission receive adequate care and attention.
Enhance Hospital Reputation: Demonstrate a commitment to patient well-being and quality of care by actively working to prevent readmissions.


The first step is to explore the data to understand its structure, contents, and potential issues that need to be addressed before proceeding with analysis. Given that the data is in a CSV file, we will use pandas to read it and display the first 5 rows and all columns.

The dataset contains information about patient encounters, demographics, medications, diagnoses, and readmission status. Many columns are already of type int64, which is suitable for our analysis. However, some columns that should be numeric are currently classified as objects, likely due to non-numeric characters. Additionally, there are missing values represented as '?'.

Given that the columns max_glu_serum and A1Cresult have a large number of missing values, we will drop them. We will replace the '?' character with nulls in the remaining columns and then convert the relevant columns to numeric.

We will also convert the age column to an ordered categorical type for easier analysis and modeling later.

Since the data is already in a CSV format, we can directly import it into PostgreSQL. We will assume that the column names in the CSV file match the column names in the PostgreSQL table.


This project demonstrates an end-to-end machine learning pipeline for predicting hospital readmissions within 30 days for diabetes patients. The pipeline includes data preprocessing, feature engineering, model development, and evaluation, all documented and implemented using PostgreSQL, Python, and AWS SageMaker.

We start by using the Diabetes 130-US Hospitals for Years 1999-2008 dataset from the UCI Machine Learning Repository. The dataset contains detailed information about patient demographics, medical histories, diagnoses, medications, and outcomes.

Steps Overview:

Data Upload and Preprocessing with PostgreSQL:

We import the dataset into PostgreSQL for initial data cleaning and preprocessing.
The dataset is stored in a table named diabetes_data, where we handle missing values, convert data types, and ensure data integrity.
Detailed SQL scripts are provided for creating the table, importing data, and performing necessary transformations.
Exporting and Uploading to AWS SageMaker:

The cleaned and processed dataset is exported from PostgreSQL to a CSV file.
The CSV file is then uploaded to an AWS S3 bucket.
Using AWS SageMaker, we load the data from S3 into a Jupyter notebook for further analysis, visualization, and model development.
This project aims to showcase the complete workflow, from raw data to a deployed machine learning model, using a combination of SQL for data handling and Python for machine learning, all while leveraging AWS's scalable infrastructure.
