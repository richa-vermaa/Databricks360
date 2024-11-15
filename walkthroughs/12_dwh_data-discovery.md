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

2. On the Add Data screen select <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_06_add_data_upload.png)

3. Drag and drop the files into the Files section so that they are uploaded.  When finished, make sure to copy the file uploaded to paths so they can be used later.  <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_07_add_files.png)

4. You can exit the Add data screen by clicking on the SQL Editor in the left navigation menu.  

5. Next click + in the upper left to create a new query window. <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_08_add_query.png)

6. Create the table by copying the code below and running the query.

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

7. Click + again to create a new query window and run the query below.

    ```sql
    COPY INTO hive_metastore.default.nyc_yellow_taxi
    FROM (
    SELECT 
        CAST(VendorID AS BIGINT) AS VendorID,
        CAST(tpep_pickup_datetime AS timestamp) as tpep_pickup_datetime,
        CAST(tpep_dropoff_datetime AS timestamp) as tpep_dropoff_datetime,
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

8. Use the following query to determine average trip time by weekday

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
    FROM hive_metastore.default.nyc_yellow_taxi
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

9. Use the Databricks Assistant to change the above query to avg distance by weekday. 