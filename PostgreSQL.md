**Step 1: Creating the 'diabetes_data' Table**
Purpose:

The initial step in my project involves establishing the foundation for data storage and analysis by creating a table in the PostgreSQL database. This table, named 'diabetes_data', will serve as the repository for all the information I extract from the dataset related to patient demographics, medical histories, diagnoses, medications, and outcomes. I designed the diabetes_data table to capture the relevant attributes present in the dataset. Each column is carefully selected to align with the project's goal of predicting hospital readmissions.  Here's the SQL code used to create the table:

SQL Query:
CREATE TABLE diabetes_data (
    encounter_id INT,
    patient_nbr INT,
    race VARCHAR(255),
    gender VARCHAR(255),
    age VARCHAR(255),
    weight VARCHAR(255),
    admission_type_id INT,
    discharge_disposition_id INT,
    admission_source_id INT,
    time_in_hospital INT,
    payer_code VARCHAR(255),
    medical_specialty VARCHAR(255),
    num_lab_procedures INT,
    num_procedures INT,
    num_medications INT,
    number_outpatient INT,
    number_emergency INT,
    number_inpatient INT,
    diag_1 VARCHAR(255),
    diag_2 VARCHAR(255),
    diag_3 VARCHAR(255),
    number_diagnoses INT,
    max_glu_serum VARCHAR(255),
    A1Cresult VARCHAR(255),
    metformin VARCHAR(255),
    repaglinide VARCHAR(255),
    nateglinide VARCHAR(255),
    chlorpropamide VARCHAR(255),
    glimepiride VARCHAR(255),
    acetohexamide VARCHAR(255),
    glipizide VARCHAR(255),
    glyburide VARCHAR(255),
    tolbutamide VARCHAR(255),
    pioglitazone VARCHAR(255),
    rosiglitazone VARCHAR(255),
    acarbose VARCHAR(255),
    miglitol VARCHAR(255),
    troglitazone VARCHAR(255),
    tolazamide VARCHAR(255),
    examide VARCHAR(255),
    citoglipton VARCHAR(255),
    insulin VARCHAR(255),
    glyburide_metformin VARCHAR(255),
    glipizide_metformin VARCHAR(255),
    glimepiride_pioglitazone VARCHAR(255),
    metformin_rosiglitazone VARCHAR(255),
    metformin_pioglitazone VARCHAR(255),
    change VARCHAR(255),
    diabetesMed VARCHAR(255),
    readmitted VARCHAR(255)
);

**Step 2: Importing Data into diabetes_data Table**

Now that the 'diabetes_data' table structure is defined, I'll populate it with the data from the dataset. This step is crucial as it sets the stage for subsequent data analysis and machine learning modeling. I'll use PostgreSQL's powerful COPY command for efficient data loading from a CSV file.

SQL Query:
COPY diabetes_data 
FROM '/Users/carlos72386/Downloads/diabetes-dataset/diabetic_data.csv' 
DELIMITER ',' 
CSV HEADER;

**Step 3: Verifying Data Import**

After importing the data using the COPY command, it's essential to verify that the data has been loaded correctly into the 'diabetes_data' table. This step helps ensure data integrity and identify any potential issues early in the pipeline.

SQL Query:
SELECT *
FROM diabetes_data
LIMIT 10

Step 4: Confirming the Number of Rows Imported

After importing the data, it's crucial to confirm that the correct number of rows has been transferred into the 'diabetes_data' table. This step helps ensure data completeness and verification that the import process was successful.

SQL Query:

SQL
SELECT COUNT(*)
FROM diabetes_data

The output returned 101,766 rows, which tells me the data has been successfully imported

