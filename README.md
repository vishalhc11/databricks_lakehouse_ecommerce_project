# databricks_lakehouse_ecommerce_project
Ecommerce project for practice - Assignment 2

This project is dividend into 7 tasks.

## 1.  Task_1_Raw_Data_Ingestion.ipynb  :  
Reading CSV files from Managed Volumes (/Volumes/main/ecommerce/lakehouse_volumes/raw_dataset/) and writing parquet file to (/Volumes/main/ecommerce/lakehouse_volumes/bronze_dataset/)  


## 2.  Task_2_Silver_Layer_Data_Standarization.ipynb
Reading parquet files for each entity into different dataframes. Enforcing schema as per the data in CSV for each entity. Perform cleaning. Created delta tables and written clean datasets to SILVER/
Problem : error occured while enforcing schema. Some of the features 
  
  - reading data from parquet file gives error ---"Expected Spark type integer, actual Parquet type BYTE_ARRAY. SQLSTATE: KD001" . This error can be solved by setting configuration of spark which is not available       in databricks free editions. 
  - This does not work in Serverless cluster - Databricks free edition
  - Disable vectorized reader to allow Spark to handle the string-to-timestamp conversion
  - spark.conf.set("spark.sql.parquet.enableVectorizedReader", "false")

Solution for free editon : casted each columns to their datatytes after reading parquet data files/ reading raw dataframes from Task_1_Raw_Data_Ingestion notebook.

Cleaning 
- Drop duplicates on key columns
- Drop rows when key columns are null
- Added necessary columns like LOAD_DATE
- Casting each columns to their data types
- Added necessary columns for dimension tables for SCD 2 implementation

## 3. Task_3_SCD_2_implementation.ipynb : 

  -- SCD 2 implemented for customer. Created test case to perform SCD 2 implementation


## 4. Task_4_Fact_Table_Creation.ipynb

  --  Aggregated Order_payments data to get total amount for each order ids. Used this data to create Fact_Sales table.

  -- Join datasets : orders, order_items, ordrer_payment_aggregated_data, customer, product 


## 5. Task_5_Gold_Business_aggregation.ipynb

  -- Sales by state or region
  -- Top products by revenue
  -- Daily or monthly sales trends


## 6. Task_6_DataQuality_Validation.ipynb

  -- CHECK 1: ROW COUNT CHECK (Are the tables empty?)
  -- CHECK 2: NULL CHECK (Do we have Null Primary Keys?)
  -- CHECK 3: BUSINESS LOGIC CHECK (Negative Revenue?)

  
## 7. Task_7_Delta_Lake_Time_Travel_Rollback.ipynb

  -- Performed Time Travel for customer datasets and 


## 8. Task_8_Master_Pipeline.ipynb

  -- Called all notebooks sequentially using dbutils.run.notebook() to create pipeline

