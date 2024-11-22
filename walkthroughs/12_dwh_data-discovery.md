# Query Data in a Databricks SQL Warehouse

This exercise uses the NYC Taxi Open Dataset that cound be found at [NYC Taxi and Limousine yellow dataset - Azure Open Datasets | Microsoft Learn](https://learn.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets).

For this specific example we are using the January-April 2024 Yellow Taxi Trip Records (Parquet) files that can be found at [TLC Trip Record Data - TLC](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). 

Please download the following files:
* [January 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet)
* [February 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-02.parquet)
* [March 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-03.parquet)
* [April 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-04.parquet)


1. Our first step is to add the January - April 2024 Yellow Taxi Trip Records files to the Databricks File Store (DBFS).  To do so, go to Catalog and click + (Add Data)  <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_05_add_data.jpeg)<BR>&nbsp;<BR>
NOTE: For this lab we are using the DBFS for ease of use.  In most customer scenarios, you would upload the files to an external Volume within Unity Catalog. <BR>&nbsp;<BR>
<br>

2. On the Add Data screen select <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_06_add_data_upload.png)
<br>

3. Drag and drop the files into the Files section so that they are uploaded.  When finished, make sure to copy the file uploaded to paths so they can be used later.  <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_07_add_files.png)
<br>

4. You can exit the Add data screen by clicking on the SQL Editor in the left navigation menu.  
<br>

5. Next click + in the upper left to create a new query window. <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_08_add_query.png)
<br>

6. Create the table manually by copying the code below and running the query.

    ```sql
    DROP TABLE IF EXISTS  hive_metastore.default.nyc_yellow_taxi;

    CREATE TABLE  hive_metastore.default.nyc_yellow_taxi ( 
        VendorID BIGINT, 
        tpep_pickup_datetime TIMESTAMP_NTZ, 
        tpep_dropoff_datetime TIMESTAMP_NTZ, 
        passenger_count DOUBLE, 
        trip_distance DOUBLE, 
        RatecodeID DOUBLE, 
        store_and_fwd_flag STRING, 
        PULocationID BIGINT, 
        DOLocationID BIGINT, 
        payment_type BIGINT, 
        fare_amount DOUBLE, 
        extra DOUBLE, 
        mta_tax DOUBLE, 
        tip_amount DOUBLE, 
        tolls_amount DOUBLE, 
        improvement_surcharge DOUBLE, 
        total_amount DOUBLE, 
        congestion_surcharge DOUBLE, 
        airport_fee DOUBLE 
    );
    ```
<br>

7. Click + again to create a new query window and run the query below to manually populate the table.

    ```sql
    COPY INTO hive_metastore.default.nyc_yellow_taxi
    FROM (
    SELECT 
        CAST(VendorID AS BIGINT) AS VendorID,
        CAST(tpep_pickup_datetime AS TIMESTAMP_NTZ) as tpep_pickup_datetime,
        CAST(tpep_dropoff_datetime AS TIMESTAMP_NTZ) as tpep_dropoff_datetime,
        CAST(passenger_count AS DOUBLE) as passenger_count,
        trip_distance,
        CAST(RatecodeID as DOUBLE) as RatecodeID,
        store_and_fwd_flag,
        CAST(PULocationID AS BIGINT) AS PULocationID, 
        CAST(DOLocationID AS BIGINT) AS DOLocationID, 
        payment_type,
        fare_amount,
        extra,
        mta_tax,
        tip_amount,
        tolls_amount,
        improvement_surcharge,
        total_amount,
        congestion_surcharge,
        airport_fee
    FROM '/FileStore/tables/'
    )
    FILEFORMAT = PARQUET 
    FORMAT_OPTIONS ('mergeSchema' = 'true') 
    COPY_OPTIONS ('mergeSchema' = 'true');
    ```
    NOTE:  This assumes all your .parquet files are in /FileStore/tables/ and you want to copy all of them. If you only want to copy the specified files, ensure they are the only .parquet files in the directory or adjust the path and pattern to specifically match the files you're interested in.
<br>

8. Create table without having to specify a schema. All files in the folder should have same schema for the table creation to be successful. 
   ```sql
    CREATE TABLE  hive_metastore.default.nyc_yellow_taxi_auto
    USING PARQUET
    LOCATION '/FileStore/tables/';
   ```
   Run below command to validate the table properties.
   ```sql
    DESCRIBE EXTENDED hive_metastore.default.nyc_yellow_taxi_auto;
   ```
   NOTE: Scroll through the properties to verify that the table type is EXTERNAL.
<br>

9. Drop the table created in the previous step
    ```sql
    DROP TABLE hive_metastore.default.nyc_yellow_taxi_auto;
    ```
<br>
   
10. Create a Delta table using table created in steps 6 & 7 to run data manipulation queries. 
   ```sql
    CREATE TABLE  hive_metastore.default.nyc_yellow_taxi_delta
    USING DELTA
    AS SELECT * FROM  hive_metastore.default.nyc_yellow_taxi;
   ``` 
   Run below command to validate the table properties.
   ```sql
    DESCRIBE EXTENDED hive_metastore.default.nyc_yellow_taxi_delta;
   ```
   NOTE: Scroll through the properties to verify that the table type is MANAGED.
<br>
   
11. Select any 10 rows from table 
    ```sql
    SELECT * FROM hive_metastore.default.nyc_yellow_taxi_delta LIMIT 10;
    ```
<br>

12. Use the following query to determine average trip time by weekday
    ```sql
    SELECT 
    CASE 
        WHEN dayofweek(tpep_pickup_datetime) = 1 THEN 'Sunday'
        WHEN dayofweek(tpep_pickup_datetime) = 2 THEN 'Monday'
        WHEN dayofweek(tpep_pickup_datetime) = 3 THEN 'Tuesday'
        WHEN dayofweek(tpep_pickup_datetime) = 4 THEN 'Wednesday'
        WHEN dayofweek(tpep_pickup_datetime) = 5 THEN 'Thursday'
        WHEN dayofweek(tpep_pickup_datetime) = 6 THEN 'Friday'
        WHEN dayofweek(tpep_pickup_datetime) = 7 THEN 'Saturday'
    END AS weekday,
    AVG(unix_timestamp(tpep_dropoff_datetime) - unix_timestamp(tpep_pickup_datetime)) / 60 AS avg_trip_time_minutes
    FROM hive_metastore.default.nyc_yellow_taxi_delta
    GROUP BY 
    CASE 
        WHEN dayofweek(tpep_pickup_datetime) = 1 THEN 'Sunday'
        WHEN dayofweek(tpep_pickup_datetime) = 2 THEN 'Monday'
        WHEN dayofweek(tpep_pickup_datetime) = 3 THEN 'Tuesday'
        WHEN dayofweek(tpep_pickup_datetime) = 4 THEN 'Wednesday'
        WHEN dayofweek(tpep_pickup_datetime) = 5 THEN 'Thursday'
        WHEN dayofweek(tpep_pickup_datetime) = 6 THEN 'Friday'
        WHEN dayofweek(tpep_pickup_datetime) = 7 THEN 'Saturday'
    END
    ORDER BY weekday;
    ```
<br>

13. Use the Databricks Assistant to change the above query to avg distance by weekday. 
<br>

14. Use the Databricks Assistant to change the above query to avg tip amount by weekday. 
<br>

15. Delete records from Delta table
    ```sql
    DELETE FROM hive_metastore.default.nyc_yellow_taxi_delta WHERE VendorID = 1;
    ```
<br>

16. Insert more data into Delta table
    ```sql
    INSERT INTO hive_metastore.default.nyc_yellow_taxi_delta
    select * from hive_metastore.default.nyc_yellow_taxi WHERE VendorID = 1;
    ```
<br>

17. Update records in Delta table
    ```sql
    UPDATE hive_metastore.default.nyc_yellow_taxi_delta
    SET tip_amount = 5
    where VendorID = 1 and DOLocationID = 7;
    ```
<br>

18. Use the Databricks Assistant to write a merge query to hive_metastore.default.nyc_yellow_taxi_delta from hive_metastore.default.nyc_yellow_taxi where vendorID = 1 and DOLocationID = 7
<br>
    


19. Use following query to determine Pickup Hour Distribution of Taxi rides 
    ```sql
    SELECT
       HOUR(tpep_pickup_datetime) AS pickup_hour,
       COUNT(1) AS number_of_rides
    FROM
       hive_metastore.default.nyc_yellow_taxi
    GROUP BY
       HOUR(tpep_pickup_datetime)
    ORDER BY
       pickup_hour;
    ```
<br>

20. Use following query to determine Monthly Total Fare Amounts 
    ```sql
    SELECT
       YEAR(tpep_pickup_datetime) AS year,
       MONTH(tpep_pickup_datetime) AS month,
       SUM(ROUND(fare_amount,2)) AS total_fare
    FROM
       hive_metastore.default.nyc_yellow_taxi
    GROUP BY
        YEAR(tpep_pickup_datetime),
        MONTH(tpep_pickup_datetime)
    ORDER BY
       year DESC, month DESC
    LIMIT 5;
    ```

   NOTE: ROUND function might not work. Use assistant and ask to fix the rounding in the query.