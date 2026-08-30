**Enterprise Retail Data Lakehouse**

An end-to-end retail data engineering project built using Databricks, PySpark, Spark SQL, Delta Lake, Databricks Workflows, and PostgreSQL.

The project processes the Olist Brazilian E-Commerce dataset through a Medallion Architecture, performs data cleaning and validation, transforms the data using Spark SQL, builds a Gold-layer Star Schema, and loads the final analytical tables into PostgreSQL.

**Project Overview**

This project demonstrates the development of an end-to-end batch data engineering pipeline for retail/e-commerce data.

The pipeline takes raw CSV datasets, ingests them into Databricks, stores them as Bronze Delta tables, cleans and validates the data in the Silver layer, performs SQL-based transformations, creates a dimensional Star Schema in the Gold layer, and finally loads the analytical tables into PostgreSQL.

**Pipeline**

Olist CSV Files -> Databricks Volume -> Bronze Layer-Raw Delta Tables -> Silver Layer-Data Cleaning -> Data Validation-Quality Checks -> Transformation-Spark SQL -> Gold Layer-Star Schema -> PostgreSQL-Analytics Layer

**MEDALLION ARCHITECTURE**

Raw CSV -> BRONZE-Raw -> SILVER-Cleaned-Validated -> GOLD-Business-Model ->PostgreSQL

**Technologies Used:** 

Python : Pipeline development,

PySpark : Data ingestion and cleaning,

Spark SQL : Data transformation and modeling,

Databricks : Data engineering and lakehouse platform,

Delta Lake : Bronze, Silver and Gold table storage,

Databricks Workflows : Pipeline orchestration,

PostgreSQL(neon) : Final analytical database

**Dataset**

The project uses the Olist Brazilian E-Commerce dataset.

The dataset contains information about customers, orders, products, sellers, payments, reviews and locations.

Source datasets : 

olist_customers_dataset.csv,

olist_orders_dataset.csv,

olist_order_items_dataset.csv,

olist_products_dataset.csv,

olist_sellers_dataset.csv,

olist_order_payments_dataset.csv,

olist_order_reviews_dataset.csv,

olist_geolocation_dataset.csv,

product_category_name_translation.csv

**Bronze Layer**

The Bronze layer contains the raw ingested data.

The CSV files are read into Spark DataFrames and stored as Delta tables in Databricks.

Bronze Tables : 

bronze_customers,

bronze_orders,

bronze_order_items,

bronze_products,

bronze_sellers,

bronze_payments,

bronze_reviews,

bronze_geolocation,

bronze_category_translation

**Ingestion Process**

The ingestion pipeline performs:

1.Reads CSV files from the Databricks data location.

2.Loads the files into Spark DataFrames.

3.Uses the CSV header to identify columns.

4.Infers the input schema.

5.Creates Bronze Delta tables.

6.Stores the raw datasets for downstream processing.

**Silver Layer**

The Silver layer contains cleaned and standardized data.

Data cleaning is implemented using PySpark.

**Cleaning Operations**

The cleaning pipeline performs:

1.Duplicate removal

2.Null handling

3.String trimming

4.Text standardization

5.Case standardization

6.Numeric validation

7.Timestamp conversion

8.Invalid-value filtering

9.Review score validation

10.Payment validation

11.Product attribute cleaning

12.Location standardization

Silver Tables :

silver_customers,

silver_orders,

silver_order_items,

silver_products,

silver_sellers,

silver_payments,

silver_reviews,

silver_geolocation,

silver_category_translation

**Data Validation**

Data validation is performed after the cleaning stage.

The validation process checks whether the Silver datasets satisfy important data-quality conditions before they are used for downstream transformations.

Validation Checks :

1.Duplicate records

2.Null values

3.Primary key validity

4.Foreign key consistency

5.Invalid prices

6.Invalid freight values

7.Invalid payment values

8.Invalid review scores

9.Date and timestamp validity

10.Delivery-date consistency

**Data Transformation**

The cleaned Silver datasets are transformed using Spark SQL.

Transformation Operations

The transformation layer performs operations such as:

1.Joining related datasets

2.Aggregating order information

3.Calculating sales metrics

4.Calculating delivery metrics

5.Creating customer-level information

6.Preparing product-level information

7.Preparing seller-level information

8.Processing payment information

9.Date-based transformations

10.Window-function based calculations

**Gold Layer**

The Gold layer contains business-ready analytical tables.

A Star Schema is implemented for analytical querying.

**Star Schema**

            dim_customer 
            
                   |
                   v

 dim_date  -->  fact_sales  <-- dim_product 
 
                  |
                  
     +------------+------------+
     
       |                   |
       
       v                   v
       
  dim_seller          dim_payment 

**Fact Table**

fact_sales

The central fact table stores sales-related transactional data.

**Grain**

One row represents one order item.

This means each row in fact_sales represents an individual product item within an order.

The fact table connects to the dimension tables using dimension keys.

**Dimension Tables**

dim_customer,

dim_product,

dim_seller,

dim_date,

dim_payment

**Databricks Workflow**

The complete data pipeline is orchestrated using Databricks Workflows.

**Workflow Execution**

01_Data_Ingestion -> 02_Data_Cleaning -> 03_Data_Validation -> 04_Data_Transformation -> 05_Star_Schema -> 06_Load_to_PostgreSQL

Each stage is executed according to the workflow dependencies.

Workflow Benefits :

Automated notebook execution,

Task dependency management,

Centralized monitoring,

Failure detection,

Repeatable pipeline execution,

Easier maintenance of ETL processes

**PostgreSQL**

The Gold-layer tables are loaded into PostgreSQL using JDBC connectivity.

PostgreSQL acts as the final relational analytical layer.

Final PostgreSQL tables :

dim_customer,

dim_product,

dim_seller,

dim_date,

dim_payment,

fact_sales
