This exercise uses the NYC Taxi Open Dataset that cound be found at [NYC Taxi and Limousine yellow dataset - Azure Open Datasets | Microsoft Learn](https://learn.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets).

For this specific example we are using the January-April 2024 Yellow Taxi Trip Records (Parquet) files that can be found at [TLC Trip Record Data - TLC](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). 

Please download the following files:
* [January 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet)
* [February 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-02.parquet)
* [March 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-03.parquet)
* [April 2024 - Yellow Taxi Trip Records (PARQUET)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-04.parquet)


1) Our first step is to add the January - April 2024 Yellow Taxi Trip Records files to the Databricks File Store (DBFS).  To do so, go to Catalog and click + (Add Data)  <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_05_add_data.jpeg)<BR>&nbsp;<BR>
NOTE: For this lab we are using the DBFS for ease of use.  In most customer scenarios, you would upload the files to an external Volume within Unity Catalog. <BR>&nbsp;<BR>

2) On the Add Data screen select <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_06_add_data_upload.png)

3) Drag and drop the files into the Files section so that they are uploaded.  When finished, you can exit the screen.  <BR>&nbsp;<BR>
![picture alt](/imagery/dwh_07_add_files.png)

4) We are not going to run the notebook, but we will need these file path for our COPY INTO command in a later step

```sql
CREATE TABLE default.nyc_yellow_taxi (
doLocationId string,
endLat double,
endLon double,
extra double,
fareAmount double,
improvementSurcharge string,
mtaTax double,
passengerCount int,
paymentType string,
puLocationId string,
puMonth int,
puYear int,
rateCodeId int,
startLat double,
startLon double,
storeAndFwdFlag string,
tipAmount double,
tollsAmount double,
totalAmount double,
tpepDropoffDateTime timestamp,
tpepPickupDateTime timestamp,
tripDistance double,
vendorID int
);
```

```sql

COPY INTO  default.nyc_yellow_taxi2
FROM 'dbfs:/Workspace/Users/admin@mngenvmcap230221.onmicrosoft.com/data/yellow_tripdata_2023-01.parquet'
FILEFORMAT = PARQUET
FORMAT_OPTIONS ('mergeSchema' = 'true')
COPY_OPTIONS ('mergeSchema' = 'true');
```