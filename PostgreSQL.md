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

### Step 6: Handling Missing Values

Identifying and addressing missing values ensures data integrity and prepares it for analysis. Given the high percentage of missing values in the max_glu_serum and A1Cresult columns, I've decided to drop them from the analysis. The remaining columns don't have significant missing values, so no further imputation is necessary.

Columns max_glu_serum and A1Cresult have a very high percentage of missing values (94.75% and 83.28%, respectively). Keeping these columns would not add meaningful information to the model due to the significant lack of data. Dropping them helps reduce noise and complexity in the dataset.

Replacing missing values in categorical columns with 'Unknown' is a common practice in data preprocessing, serving several key purposes. Firstly, it maintains data integrity by ensuring that every record has a value, preventing errors or issues during subsequent analysis and modeling. Secondly, it makes the dataset compatible with many machine learning algorithms that require complete data and cannot handle null values directly. Thirdly, the 'Unknown' category can provide valuable insights during analysis, highlighting areas where data collection may need improvement or uncovering patterns related to missing information. Finally, it avoids the potential bias that could be introduced by imputing missing categorical values with statistical measures like mode or median, as these might not accurately represent the missing data.

Assigning a value of 0 to missing weight values indicates that the weight data was not recorded or is not applicable. This is a simple imputation strategy that avoids introducing artificial data.

Imputing missing values in numerical columns with the median is a robust strategy for several reasons. The median, as the middle value in a sorted dataset, is less sensitive to extreme values or outliers compared to the mean. This stability makes it a reliable choice for filling in missing data points while preserving the overall statistical properties of the dataset. By using the median, I can ensure that the imputed values are consistent with the central tendency of the existing data, avoiding the introduction of artificial extremes that could distort the analysis. This approach also helps maintain the integrity of the dataset, potentially leading to improved performance of machine learning models trained on this data, as the underlying distribution is not significantly altered.

These are all the steps I took in the following SQL query:

#### SQL Query:

-- Drop columns with high percentage of missing values
- ALTER TABLE diabetes_data
- DROP COLUMN max_glu_serum,
- DROP COLUMN A1Cresult;

-- Replace missing values in categorical columns with 'Unknown'
- UPDATE diabetes_data
- SET race = 'Unknown'
- WHERE race IS NULL OR race = '';

- UPDATE diabetes_data
- SET payer_code = 'Unknown'
- WHERE payer_code IS NULL OR payer_code = '';

- UPDATE diabetes_data
- SET medical_specialty = 'Unknown'
- WHERE medical_specialty IS NULL OR medical_specialty = '';

- UPDATE diabetes_data
- SET gender = 'Unknown'
- WHERE gender IS NULL OR gender = '';

-- Replace missing values in 'weight' with 0
- UPDATE diabetes_data
- SET weight = 0
- WHERE weight IS NULL;

-- Replace missing values in numerical columns with the median value
-- Replace missing values in `num_lab_procedures`
- UPDATE diabetes_data
- SET num_lab_procedures = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY num_lab_procedures) FROM diabetes_data)
- WHERE num_lab_procedures IS NULL;

-- Replace missing values in `num_procedures`
- UPDATE diabetes_data
- SET num_procedures = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY num_procedures) FROM diabetes_data)
- WHERE num_procedures IS NULL;

-- Replace missing values in `num_medications`
- UPDATE diabetes_data
- SET num_medications = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY num_medications) FROM diabetes_data)
-  WHERE num_medications IS NULL;

-- Replace missing values in `number_outpatient`
- UPDATE diabetes_data
- SET number_outpatient = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY number_outpatient) FROM diabetes_data)
- WHERE number_outpatient IS NULL;

-- Replace missing values in `number_emergency`
- UPDATE diabetes_data
- SET number_emergency = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY number_emergency) FROM diabetes_data)
- WHERE number_emergency IS NULL;

-- Replace missing values in `number_inpatient`
- UPDATE diabetes_data
- SET number_inpatient = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY number_inpatient) FROM diabetes_data)
- WHERE number_inpatient IS NULL;

-- Replace missing values in `number_diagnoses`
- UPDATE diabetes_data
- SET number_diagnoses = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY number_diagnoses) FROM diabetes_data)
- WHERE number_diagnoses IS NULL;

-- Replace missing values in `time_in_hospital`
- UPDATE diabetes_data
- SET time_in_hospital = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY time_in_hospital) FROM diabetes_data)
- WHERE time_in_hospital IS NULL;

-- Replace missing values in `encounter_id`
- UPDATE diabetes_data
- SET encounter_id = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY encounter_id) FROM diabetes_data)
- WHERE encounter_id IS NULL;

-- Replace missing values in `patient_nbr`
- UPDATE diabetes_data
- SET patient_nbr = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY patient_nbr) FROM diabetes_data)
- WHERE patient_nbr IS NULL;

-- Replace missing values in `admission_type_id`
- UPDATE diabetes_data
- SET admission_type_id = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY admission_type_id) FROM diabetes_data)
- WHERE admission_type_id IS NULL;

-- Replace missing values in `discharge_disposition_id`
- UPDATE diabetes_data
- SET discharge_disposition_id = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY discharge_disposition_id) FROM diabetes_data)
- WHERE discharge_disposition_id IS NULL;

-- Replace missing values in `admission_source_id`
- UPDATE diabetes_data
- SET admission_source_id = (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY admission_source_id) FROM diabetes_data)
- WHERE admission_source_id IS NULL;

-- Replace missing values in `diag_1`
- UPDATE diabetes_data
- SET diag_1 = 'Unknown'
- WHERE diag_1 IS NULL OR diag_1 = '';

-- Replace missing values in `diag_2`
- UPDATE diabetes_data
- SET diag_2 = 'Unknown'
- WHERE diag_2 IS NULL OR diag_2 = '';

-- Replace missing values in `diag_3`
- UPDATE diabetes_data
- SET diag_3 = 'Unknown'
- WHERE diag_3 IS NULL OR diag_3 = '';

### Step 7: Converting Data Types

Converting data types ensures consistency and compatibility with analysis tools. This includes converting weight to a numeric type, age to an integer, and normalizing values in the gender column.

#### SQL Query:

-- Convert 'weight' to NUMERIC
- UPDATE diabetes_data
- SET weight = NULL
- WHERE weight !~ '^\d+\.?\d*$';

- ALTER TABLE diabetes_data
- ALTER COLUMN weight TYPE NUMERIC USING (NULLIF(weight, '')::NUMERIC);

-- Convert 'age' to INT
- ALTER TABLE diabetes_data
- ALTER COLUMN age TYPE INT USING (CAST(substring(age FROM '\d+') AS INT));

-- Normalize 'gender' values and convert data type
- UPDATE diabetes_data
- SET gender = CASE
    - WHEN gender ILIKE 'male' THEN 'Male'
    - WHEN gender ILIKE 'female' THEN 'Female'
    - ELSE 'Unknown'
- END;

- ALTER TABLE diabetes_data
- ALTER COLUMN gender TYPE VARCHAR(10);

-- Convert various columns to INT
- ALTER TABLE diabetes_data
- ALTER COLUMN admission_type_id TYPE INT USING (admission_type_id::INT),
- ALTER COLUMN discharge_disposition_id TYPE INT USING (discharge_disposition_id::INT),
- ALTER COLUMN admission_source_id TYPE INT USING (admission_source_id::INT),
- ALTER COLUMN num_lab_procedures TYPE INT USING (num_lab_procedures::INT),
- ALTER COLUMN num_procedures TYPE INT USING (num_procedures::INT),
- ALTER COLUMN num_medications TYPE INT USING (num_medications::INT),
- ALTER COLUMN number_outpatient TYPE INT USING (number_outpatient::INT),
- ALTER COLUMN number_emergency TYPE INT USING (number_emergency::INT),
- ALTER COLUMN number_inpatient TYPE INT USING (number_inpatient::INT),
- ALTER COLUMN number_diagnoses TYPE INT USING (number_diagnoses::INT);

-- Replace missing values in 'payer_code' and 'medical_specialty' with 'Unknown'
- UPDATE diabetes_data
- SET payer_code = 'Unknown'
- WHERE payer_code IS NULL OR payer_code = '';

- UPDATE diabetes_data
- SET medical_specialty = 'Unknown'
- WHERE medical_specialty IS NULL OR medical_specialty = '';
