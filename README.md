# Home_Sales

# 🏡 Home Sales Analysis with Apache Spark

## Overview

This project utilizes PySpark and SparkSQL to analyze a dataset of home sales, answering key business questions, measuring performance with and without caching, and optimizing queries using partitioned Parquet files. The dataset is read from an AWS S3 bucket and manipulated using distributed computing techniques.

## Technologies Used

- Python
- Apache Spark (PySpark)
- SparkSQL
- Jupyter Notebook
- AWS S3 (data source)
- Git and GitHub

## Dataset

- **File**: `home_sales_revised.csv`
- **Source**: AWS S3 Bucket
- **Fields include**:  
  `price`, `bedrooms`, `bathrooms`, `sqft_living`, `floors`, `view`, `condition`, `grade`, `date_built`, `date_sold`

## Tasks Completed

### Data Ingestion
- Loaded the CSV file from an S3 bucket using Spark.
- Created a DataFrame and a temporary SQL table (`home_sales`) for querying.

### SQL Queries
Executed SQL queries to answer the following:

1. **Average price of 4-bedroom homes sold per year**  
   Grouped by the year of sale is as follows,

   +--------------------+----------+
|round(avg(price), 2)|year(date)|
+--------------------+----------+
|           296363.88|      2022|
|           301819.44|      2021|
|           298353.78|      2020|
|            300263.7|      2019|
+--------------------+----------+

2. **Average price of 3 bed / 3 bath homes per year built**  
   Filtered to homes with exactly 3 bedrooms and 3 bathrooms as follows,

   +--------------------+----------+
|round(avg(price), 2)|date_built|
+--------------------+----------+
|           292676.79|      2017|
|           290555.07|      2016|
|            288770.3|      2015|
|           290852.27|      2014|
|           295962.27|      2013|
|           293683.19|      2012|
|           291117.47|      2011|
|           292859.62|      2010|
+--------------------+----------+

3. **Average price for 3 bed / 3 bath / 2 floors / ≥ 2000 sqft homes**  
   Grouped by the year the home was built as follows

   +--------------------+----------+
|round(avg(price), 2)|date_built|
+--------------------+----------+
|           280317.58|      2017|
|            293965.1|      2016|
|           297609.97|      2015|
|           298264.72|      2014|
|           303676.79|      2013|
|           307539.97|      2012|
|           276553.81|      2011|
|           285010.22|      2010|
+--------------------+----------+

4. **Average home price per view rating (≥ $350,000 only)**  
   Measured query runtime for performance.

   +----+--------------------+
|view|round(avg(price), 2)|
+----+--------------------+
|  51|           788128.21|
|  54|           798684.82|
|  69|           750537.94|
|  87|           1072285.2|
|  73|           752861.18|
|  64|           767036.67|
|  59|            791453.0|
|  85|          1056336.74|
|  52|           733780.26|
|  71|            775651.1|
|  98|          1053739.33|
|  99|          1061201.42|
|  96|          1017815.92|
| 100|           1026669.5|
|  70|           695865.58|
|  61|           746877.59|
|  75|          1114042.94|
|  78|          1080649.37|
|  89|          1107839.15|
|  77|          1076205.56|
+----+--------------------+
only showing top 20 rows

--- 0.18457293510437012 seconds ---

###  Caching
- Cached the `home_sales` table.
- Reran the view-rating query using the cached data.
- Compared cached query runtime to the uncached version.

### Parquet Partitioning
- Saved the DataFrame as a partitioned Parquet file by `date_built`.
- Read the Parquet file into a new DataFrame.
- Created a new temporary table from the Parquet data.
- Reran the view-rating query on this table and compared runtime.

### Uncaching
- Uncached the `home_sales` table and verified it was successfully uncached.

