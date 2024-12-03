# SQL Alerts 

Databricks SQL Alerts enable the execution of SQL queries, the evaluation of specified conditions, and the dispatch of notifications when those conditions are satisfied.

For this exercise, we will create a SQL Query and create an alert to evaluate conditions. We will also setup a schedule for our alert to be evaluated and send notifications.

Because of the nature of our data, we are going to setup a notification if the average tip amount for a day is less then 17%.  We will hard code a date to see that our query gets triggered.  Then we will modify it for the schedule.

## Step 1 - Create the SQL Query

1. Our first step is to create our SQL Query.  To do so, click on 'Queries' in the left navigation bar and click the 'Create query' button in the upper right.

2. Rename the Query by clicking on the tab title and change it to 'SQL Alert Query'.

3. Add the following SQL statement to query window and then click 'Save'. 
    ```sql
    SELECT 
        DATE(tpep_pickup_datetime) AS pickup_date, 
        AVG(try_divide(tip_amount, fare_amount)) AS tip_percentage
    FROM nyc_yellow_taxi_delta 
    where DATE(tpep_pickup_datetime) = '2024-03-31'
    GROUP BY DATE(tpep_pickup_datetime)
    ```
    When finished, your query should look like the following:
    <BR>

    ![picture alt](/imagery/dwh_15_01_query.png)
    <br>

## Step 2 - Create the initial SQL Alert

4. Our next step is to create our SQL Alert.  To do so, click on 'Alerts' in the left navigation bar and click the 'Create alert' button in the upper right.

5. In the New alert window, name the alert 'Tips under 17%' and select our 'SQL Alert Query' in the Query field and click 'Create alert'.

6. Set the 'Trigger conditions' like the screenshot below and click 'Preview alert'.  When completed, you should see a preview states like the screenshot below.
    <BR> &nbsp;<BR>
    ![picture alt](/imagery/dwh_15_02_trigger_conditions.png)
    <BR>

7. Under Notifications, set the following values:
    a. Send notification = each time alert is evaluated.
    b. Check the Send notification box

8. Leave 'Use default template' selected under Template.

9. When finished, your alert will look like the screen below.  Click 'Create alert' to save it.
    <BR> &nbsp;<BR>
    ![picture alt](/imagery/dwh_15_03_save.png)
    <BR>

<BR>

## Step 3 - Schedule the SQL Alert
We will now modify our query so it will evaluate the previous day's records and schedule it to run daily.

10.  Click on 'Queries' in the left navigation bar and select the 'SQL Alert query'.

For more on this topic check out [What are Databricks SQL Alerts](https://learn.microsoft.com/en-us/azure/databricks/sql/user/alerts/)