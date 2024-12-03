# SQL Dashboards 

Dashboards can be utilized to construct data visualizations and disseminate reports within your team. AI/BI dashboards offer AI-assisted authoring, an advanced visualization library, and an optimized configuration experience, enabling rapid transformation of data into shareable insights.

For this exercise, we will expand on the scope of the queries we did in the Data Discovery lab to build our dashboards to visualize the following fields in the 'nyc_yellow_taxi_delta' table.
* Passenger Count
* Trip Time
* Trip Distance
* Fare Amount
* Tip Percentage

1. To create a Dashboard, go to 'Dashboards' item in the left navigation bar and then click the Create Dashboard button in the upper right.<BR>
NOTE: By default, your new dashboard is automatically named with its creation timestamp and stored in your /Workspace/Users/<username> directory.

2. Click on the Dashboard title in the top center and rename your dashboard to 'NYC Taxi Cab Analysis'.  When finished, your screen should look like the following.
<BR>&nbsp;<BR>
![picture alt](/imagery/dwh_14_01_dashboard_name.png)
<br>

3. Let's define our dataset by clicking on the Data tab in the upper left and click the '+ Create from SQL' button and add the following code:
    ```sql
    SELECT 
        CASE 
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 0 AND 5 THEN '1-Early Morning'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 6 AND 9 THEN '2-Morning Rush Hour'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 10 AND 11 THEN '3-Morning'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 12 AND 15 THEN '4-Afternoon'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 16 AND 18 THEN '5-Evening Rush Hour'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 19 AND 23 THEN '6-Night'
        END AS time_of_day,
        CONCAT(EXTRACT(DAYOFWEEK FROM tpep_pickup_datetime), '-', DATE_FORMAT(tpep_pickup_datetime, 'E'))  AS day_of_week,
        ROUND(AVG(passenger_count), 2) AS avg_passenger_count,
        ROUND(AVG((unix_timestamp(tpep_dropoff_datetime) - unix_timestamp(tpep_pickup_datetime)) / 60), 1) AS avg_trip_time_minutes,
        ROUND(AVG(trip_distance), 2) AS avg_trip_distance,
        ROUND(AVG(fare_amount), 2) AS avg_fare_amount,
        ROUND(AVG(try_divide(tip_amount, fare_amount)), 2) * 100 AS avg_tip_percentage
        FROM default.nyc_yellow_taxi_delta
        GROUP BY 
        CONCAT(EXTRACT(DAYOFWEEK FROM tpep_pickup_datetime), '-', DATE_FORMAT(tpep_pickup_datetime, 'E')),
        CASE 
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 0 AND 5 THEN '1-Early Morning'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 6 AND 9 THEN '2-Morning Rush Hour'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 10 AND 11 THEN '3-Morning'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 12 AND 15 THEN '4-Afternoon'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 16 AND 18 THEN '5-Evening Rush Hour'
            WHEN HOUR(tpep_pickup_datetime) BETWEEN 19 AND 23 THEN '6-Night'
        END
        ORDER BY 
        day_of_week, time_of_day
    ```

4. Name your dataset 'NYC Taxi Summarized'.  When finished, your dataset should look like the following:
<BR>&nbsp;<BR>
![picture alt](/imagery/dwh_14_02_dataset.png)
<br>

5. Next click on the 'Canvas tab' in the upper left of the dashboard window.  We have a few options when working within the canvas window:
<BR>&nbsp;<BR>
![picture alt](/imagery/dwh_14_03_canvas.png)
<br>

6. Lets add a Pivot table.  Click 'Add a visualization' and add it to the upper left section of the window and set the following properties:<BR>
    a. Visualization: Pivot<BR>
    b. Columns: time_of_day<BR>
    c. Values<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;i. AVG(avg_passenger_count)<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;ii. AVG(avg_fare_amount)<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;iii. AVG(avg_tip_percentage)<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;iv. AVG(avg_trip_distance)<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;v. AVG(avg_trip_time_minutes)<BR>
    NOTE:  The values will first show as SUM(field), click on each SUM(field) to change it to AVG.
    <BR>&nbsp;<BR>
    When finished, your visual should look like the following: 
    <BR>&nbsp;<BR>
    ![picture alt](/imagery/dwh_14_04_pivot.png)
    <br>


7. Lets add a Line chart.  Click 'Add a visualization' and add it to the lower left section of the canvas and set the following properties:<BR>
    a. Visualization: Line<BR>
    b. X axis: time_of_day<BR>
    c. Y axis<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;ii. AVG(avg_fare_amount)<BR>
    &nbsp;&nbsp;&nbsp;&nbsp;iv. AVG(avg_trip_distance)<BR>
    d. Color by Y-series: Change 'avg_fare_amount' to green<BR>
    e. Labels: Enabled
    <BR>&nbsp;<BR>
    When finished, your visual should look like the following: 
    <BR>&nbsp;<BR>
    ![picture alt](/imagery/dwh_14_05_line.png)
    <br>

8. Lets add a bar chart via the Databricks Assistant.  Click 'Add a visualization' and add it to the lower right section of the canvas and ask the following of the Databricks Assistant; 'Create a bar chart by avg trip distance by avg tip percentage' and click Preview.  When finished, click 'Accept'.

9. Lets Add a filter based on Weekday, click 'Add a filter' and position it above the pivot table and set the following values.<BR>
    a. Filter: Multiple Values<BR>
    b. Fields: day_of_week
    <BR>&nbsp;<BR>
    When finished, your visual should look like the following: 
    <BR>&nbsp;<BR>
    ![picture alt](/imagery/dwh_14_06_filter.png)
    <br>

10. Use the filter to identify differences/ between week days and weekend days.

11. Go to the Data tab and ask the Databricks assistant to add Month to your dataset by utilizing the following statement 'Add Month to the NYC Taxi Summarized data set'.  

12. Update your dataset with the Databricks Assistant output. 

13. Go to the Canvas tab and on the Bar chart, perform the following actions:<BR>
    a. Change the x axis value to 'month'.<BR>
    b. Filter the Dataset to only January, February, March and April.
    <BR>&nbsp;<BR>
    When finished, your visual should look like the following: 
    <BR>&nbsp;<BR>
    ![picture alt](/imagery/dwh_14_07_barchart.png)
    <br>

14. Rename the canvas page to 'Summary'

15. Last of all, click 'Publish' to publish and share your Dashboard.  On the Publish NYC Taxi Cab Analysis page, Enable Genie on the bottom and click Publish.<BR>&nbsp;<BR>
    ![picture alt](/imagery/dwh_14_08_publish.png)
    <br>

16.  You have 2 refresh options, you can refresh it manually or via the Schedule button.  Both are available in the upper right of the dashboard.  Check out both to see all the options available.<BR>
For more on this topic check out [Manage scheduled dashboard updates and subscriptions](https://learn.microsoft.com/en-us/azure/databricks/dashboards/schedule-subscribe)