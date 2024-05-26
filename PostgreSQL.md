## Data Cleaning & Preparation in PostgreSQL

To prepare the 'diabetes_data' dataset for modeling, data cleaning and preprocessing will be performed directly in PostgreSQL.

### Step 1: Creating the 'diabetes_data' Table

The initial step in my project involves establishing the foundation for data storage and analysis by creating a table in the PostgreSQL database. This table, named 'diabetes_data', will serve as the repository for all the information extracted from the dataset related to patient demographics, medical histories, diagnoses, medications, and outcomes. The structure of the diabetes_data table captures the relevant attributes present in the dataset, aligning with the project's goal of predicting hospital readmissions.

#### SQL Query:

- CREATE TABLE diabetes_data (
   - encounter_id INT,
    - patient_nbr INT,
    - race VARCHAR(255),
    - gender VARCHAR(255),
    - age VARCHAR(255),
    - weight VARCHAR(255),
    - admission_type_id INT,
    - discharge_disposition_id INT,
    - admission_source_id INT,
    - time_in_hospital INT,
    - payer_code VARCHAR(255),
    - medical_specialty VARCHAR(255),
    - num_lab_procedures INT,
    - num_procedures INT,
    - num_medications INT,
    - number_outpatient INT,
    - number_emergency INT,
    - number_inpatient INT,
    - diag_1 VARCHAR(255),
    - diag_2 VARCHAR(255),
    - diag_3 VARCHAR(255),
    - number_diagnoses INT,
    - max_glu_serum VARCHAR(255),
    - A1Cresult VARCHAR(255),
    - metformin VARCHAR(255),
    - repaglinide VARCHAR(255),
    - nateglinide VARCHAR(255),
    - chlorpropamide VARCHAR(255),
    - glimepiride VARCHAR(255),
    - acetohexamide VARCHAR(255),
    - glipizide VARCHAR(255),
    - glyburide VARCHAR(255),
    - tolbutamide VARCHAR(255),
    - pioglitazone VARCHAR(255),
    - rosiglitazone VARCHAR(255),
    - acarbose VARCHAR(255),
    - miglitol VARCHAR(255),
    - troglitazone VARCHAR(255),
    - tolazamide VARCHAR(255),
    - examide VARCHAR(255),
    - citoglipton VARCHAR(255),
    - insulin VARCHAR(255),
    - glyburide_metformin VARCHAR(255),
    - glipizide_metformin VARCHAR(255),
    - glimepiride_pioglitazone VARCHAR(255),
    - metformin_rosiglitazone VARCHAR(255),
    - metformin_pioglitazone VARCHAR(255),
    - change VARCHAR(255),
    - diabetesMed VARCHAR(255),
    - readmitted VARCHAR(255)
- );

### Step 2: Importing Data into the 'diabetes_data' Table

Once the table structure is defined, I populate it with data from the dataset using PostgreSQL's COPY command for efficient data loading from a CSV file.

#### SQL Query:

- COPY diabetes_data 
- FROM '/Users/carlos72386/Downloads/diabetes-dataset/diabetic_data.csv' 
- DELIMITER ',' 
- CSV HEADER;

### Step 3: Verifying Data Import

After importing the data, it's essential to verify that the data has been loaded correctly into the 'diabetes_data' table to ensure data integrity and identify any potential issues early in the pipeline.

#### SQL Query:

- SELECT *
- FROM diabetes_data
- LIMIT 10;

### Step 4: Confirming the Number of Rows Imported

Confirming the correct number of rows transferred into the 'diabetes_data' table ensures data completeness and verifies that the import process was successful.

#### SQL Query:

- SELECT COUNT(*)
- FROM diabetes_data;

The output returned 101,766 rows, indicating successful data import.

### Step 5: Assessing the Distribution of Readmission Status

Understanding the distribution of the target variable, readmitted, helps assess the class balance in the dataset, informing modeling choices.

#### SQL Query:

- SELECT readmitted, COUNT(*) AS count
- FROM diabetes_data
- GROUP BY readmitted;

Initial analysis reveals a class imbalance in the target variable, which will need to be addressed during modeling.

### Step 6: Identify Missing Values

To identify columns with missing values and the count of missing values in each column, I executed the following:

#### SQL Query:

- SELECT 
    - COUNT(*) - COUNT(race) AS race_missing,
    - COUNT(*) - COUNT(gender) AS gender_missing,
    - COUNT(*) - COUNT(age) AS age_missing,
    - COUNT(*) - COUNT(weight) AS weight_missing,
    - COUNT(*) - COUNT(admission_type_id) AS admission_type_id_missing,
    - COUNT(*) - COUNT(discharge_disposition_id) AS discharge_disposition_id_missing,
    - COUNT(*) - COUNT(admission_source_id) AS admission_source_id_missing,
    - COUNT(*) - COUNT(time_in_hospital) AS time_in_hospital_missing,
    - COUNT(*) - COUNT(payer_code) AS payer_code_missing,
    - COUNT(*) - COUNT(medical_specialty) AS medical_specialty_missing,
    - COUNT(*) - COUNT(num_lab_procedures) AS num_lab_procedures_missing,
    - COUNT(*) - COUNT(num_procedures) AS num_procedures_missing,
    - COUNT(*) - COUNT(num_medications) AS num_medications_missing,
    - COUNT(*) - COUNT(number_outpatient) AS number_outpatient_missing,
    - COUNT(*) - COUNT(number_emergency) AS number_emergency_missing,
    - COUNT(*) - COUNT(number_inpatient) AS number_inpatient_missing,
    - COUNT(*) - COUNT(diag_1) AS diag_1_missing,
    - COUNT(*) - COUNT(diag_2) AS diag_2_missing,
    - COUNT(*) - COUNT(diag_3) AS diag_3_missing,
    - COUNT(*) - COUNT(number_diagnoses) AS number_diagnoses_missing,
    - COUNT(*) - COUNT(max_glu_serum) AS max_glu_serum_missing,
    - COUNT(*) - COUNT(A1Cresult) AS A1Cresult_missing,
    - COUNT(*) - COUNT(metformin) AS metformin_missing,
    - COUNT(*) - COUNT(repaglinide) AS repaglinide_missing,
    - COUNT(*) - COUNT(nateglinide) AS nateglinide_missing,
    - COUNT(*) - COUNT(chlorpropamide) AS chlorpropamide_missing,
    - COUNT(*) - COUNT(glimepiride) AS glimepiride_missing,
    - COUNT(*) - COUNT(acetohexamide) AS acetohexamide_missing,
    - COUNT(*) - COUNT(glipizide) AS glipizide_missing,
    - COUNT(*) - COUNT(glyburide) AS glyburide_missing,
    - COUNT(*) - COUNT(tolbutamide) AS tolbutamide_missing,
    - COUNT(*) - COUNT(pioglitazone) AS pioglitazone_missing,
    - COUNT(*) - COUNT(rosiglitazone) AS rosiglitazone_missing,
    - COUNT(*) - COUNT(acarbose) AS acarbose_missing,
    - COUNT(*) - COUNT(miglitol) AS miglitol_missing,
    - COUNT(*) - COUNT(troglitazone) AS troglitazone_missing,
    - COUNT(*) - COUNT(tolazamide) AS tolazamide_missing,
    - COUNT(*) - COUNT(examide) AS examide_missing,
    - COUNT(*) - COUNT(citoglipton) AS citoglipton_missing,
    - COUNT(*) - COUNT(insulin) AS insulin_missing,
    - COUNT(*) - COUNT(glyburide_metformin) AS glyburide_metformin_missing,
    - COUNT(*) - COUNT(glipizide_metformin) AS glipizide_metformin_missing,
    - COUNT(*) - COUNT(glimepiride_pioglitazone) AS glimepiride_pioglitazone_missing,
    - COUNT(*) - COUNT(metformin_rosiglitazone) AS metformin_rosiglitazone_missing,
    - COUNT(*) - COUNT(metformin_pioglitazone) AS metformin_pioglitazone_missing,
    - COUNT(*) - COUNT(change) AS change_missing,
    - COUNT(*) - COUNT(diabetesMed) AS diabetesMed_missing,
    - COUNT(*) - COUNT(readmitted) AS readmitted_missing
- FROM diabetes_data;

All columns returned 0 across the board

### Step 7: Verify Data Types

Ensure that all columns have the correct data type for analysis. If necessary, I will alter the data to match my requirements. 

#### SQL Query:

- SELECT 
    - column_name, 
    - data_type 
- FROM 
    - information_schema.columns 
- WHERE 
    - table_name = 'diabetes_data'

 The data types seem generally correct. There are a few things, however, that I'm going to adjust, specially for some of the columns that should be numeric but are currently 'character varying'.

### Step 8: Alter Necessary Data Types

 I'm going to convert the 'age' and 'weight' columns to their appropriate numeric data types. 

#### SQL Query:

- SELECT DISTINCT weight
- FROM diabetes_data
- WHERE weight !~ '^[0-9.]+$';
 
After executing this code, I noticed that the values are in ranges, and not directly numeric. I also noticed that there were non-numeric values like '?' that need to be handled. I will proceed with this first.

#### Step 8.1: Update Non-numeri Values in the 'weight' Column

#### SQL Query:

To identify non-numeric values in the weight column:

- SELECT DISTINCT weight 
- FROM diabetes_data 
- WHERE NOT (weight ~ '^[0-9.]+$');

#### Step 8.2: Convert 'weight' ranges to Midpoints

#### SQL Query:

To replace non-numeric values ("?") with NULL and convert weight ranges to their midpoints:

- UPDATE diabetes_data
- SET weight = NULL
- WHERE weight = '?';

- UPDATE diabetes_data
- SET weight = '12.5' WHERE weight = '[0-25)';
- UPDATE diabetes_data
- SET weight = '37.5' WHERE weight = '[25-50)';
- UPDATE diabetes_data
- SET weight = '62.5' WHERE weight = '[50-75)';
- UPDATE diabetes_data
- SET weight = '87.5' WHERE weight = '[75-100)';
- UPDATE diabetes_data
- SET weight = '112.5' WHERE weight = '[100-125)';
- UPDATE diabetes_data
- SET weight = '137.5' WHERE weight = '[125-150)';
- UPDATE diabetes_data
- SET weight = '162.5' WHERE weight = '[150-175)';
- UPDATE diabetes_data
- SET weight = '187.5' WHERE weight = '[175-200)';
- UPDATE diabetes_data
- SET weight = '200' WHERE weight = '>200';

#### Step 8.3: Convert 'weight' Column to Numeric

Now that I've replaced the ranges with the midpoints, I will convert the 'weight' column to 'NUMERIC'.

#### SQL Query:

- ALTER TABLE diabetes_data
- ALTER COLUMN weight TYPE NUMERIC USING (NULLIF(weight, '')::NUMERIC);

### Step 9: Handle Missing Values in Categorical Columns

Since I had no missing values initially, I can now verify and and ensure the integrity of the categorical columns. 

#### SQL Query:

- UPDATE diabetes_data
- SET race = 'Unknown'
- WHERE race IS NULL OR race = '';

- UPDATE diabetes_data
- SET gender = 'Unknown'
- WHERE gender IS NULL OR gender = '';

- UPDATE diabetes_data
- SET payer_code = 'Unknown'
- WHERE payer_code IS NULL OR payer_code = '';

- UPDATE diabetes_data
- SET medical_specialty = 'Unknown'
- WHERE medical_specialty IS NULL OR medical_specialty = '';

- UPDATE diabetes_data
- SET diag_1 = 'Unknown'
- WHERE diag_1 IS NULL OR diag_1 = '';

- UPDATE diabetes_data
- SET diag_2 = 'Unknown'
- WHERE diag_2 IS NULL OR diag_2 = '';

- UPDATE diabetes_data
- SET diag_3 = 'Unknown'
- WHERE diag_3 IS NULL OR diag_3 = '';

### Step 10: Ensure Data Types of Other Columns

I'm going to verify and alter data types for other relevant columns to ensure they are correctly formatted for the analysis. 

Convert integer colummns:

- ALTER TABLE diabetes_data
- ALTER COLUMN num_lab_procedures TYPE INTEGER USING (num_lab_procedures::INTEGER),
- ALTER COLUMN num_procedures TYPE INTEGER USING (num_procedures::INTEGER),
- ALTER COLUMN num_medications TYPE INTEGER USING (num_medications::INTEGER),
- ALTER COLUMN number_outpatient TYPE INTEGER USING (number_outpatient::INTEGER),
- ALTER COLUMN number_emergency TYPE INTEGER USING (number_emergency::INTEGER),
- ALTER COLUMN number_inpatient TYPE INTEGER USING (number_inpatient::INTEGER),
- ALTER COLUMN number_diagnoses TYPE INTEGER USING (number_diagnoses::INTEGER);

Convert ID columns:

- ALTER TABLE diabetes_data
- ALTER COLUMN admission_type_id TYPE INTEGER USING (admission_type_id::INTEGER),
- ALTER COLUMN discharge_disposition_id TYPE INTEGER USING (discharge_disposition_id::INTEGER),
- ALTER COLUMN admission_source_id TYPE INTEGER USING (admission_source_id::INTEGER);
