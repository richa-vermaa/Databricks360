# Query Management in Databricks

This exercise is run after [Data Discovery](/walkthroughs/12_dwh_data-discovery.md) and explains how to manage SQL Queries inside Databricks.

## Save your query

1. Save the query by clicking on the Save button in the top right of your SQL Editor window 
    <img src="/imagery/dwh_save_query.jpeg" alt="Save your query" width="1000" height="auto">

2. On the 'Save to' prompt window, click on '+' to create a new folder 'queries' and provide a descriptive name for the query to save it.
    <img src="/imagery/dwh_create_new_queries_folder.jpeg" alt="Create new folder for queries" width="1000" height="auto">

3. Click on 'Queries' to view all saved queries in your Databricks workspace.
    <img src="/imagery/dwh_view_saved_queries.jpeg" alt="View saved queries" width="1000" height="auto">


## Query History

1. Click on "Query History" in the left panel to view all queries run by a user/on a compute/in selected duration of time or the statement type such as COPY, INSERT etc.
    <img src="/imagery/dwh_query_history.jpeg" alt="View Query History" width="1000" height="auto">

2. Click the name of the query from Query History to view the query details.
   <img src="/imagery/dwh_view_query_details.jpeg" alt="View query details" width="1000" height="auto">

   Here is how each of the property translates. 
   * Statement ID: This is the universally unique identifier (UUID) associated with the given query object.
   * Query status: The query is tagged with its current status: Queued, Running, Finished, Failed, or Cancelled.
   * Compute type: This field shows the compute type used for the query.
   * Query statement: This section includes the complete query statement. If the query is too long to be shown in the preview, click the Expand query Expand query icon to see the full text.
   * Query source: This field shows where the query originated. Queries can come from a variety of sources, including AI/BI dashboards, query objects, the Databricks SQL editor, notebooks, and Delta Live Tables pipelines (Public Preview).
   * Wall-clock duration: Shows the elapsed wall-clock time between the start of scheduling and the end of query execution. The total is automatically displayed as the sum of scheduling time and running time. To learn more, each of those fields can be expanded into sub-categories.
   * Summary details: The bottom of the panel includes summary details about the query’s performance, including aggregated task time, rows read and returned, files and partitions, and any spilling that might have occurred.

   For more detailed information about the query’s performance, including its execution plan, click [View Query Profile](https://learn.microsoft.com/en-us/azure/databricks/sql/user/queries/query-profile) near the bottom of the page
<br>

3. To cancel a running query, select the Query and scroll down to click Cancel Query button at the bottom right of screen.
    <img src="/imagery/dwh_cancel_running_query.jpeg" alt="Cancel running query" width="1000" height="auto">
  
Note: Query History cover queries executed through a DLT pipeline as well.


## Query Profile

Query profile is used to visualise the details of a query execution. It helps in troubleshooting the performance bottlenecks during the execution. For example:
* You can visualize each query operator and related metrics, such as the time spent, number of rows processed, rows processed, and memory consumption.
* You can identify the slowest part of a query execution at a glance and assess the impacts of modifications to the query.
* You can discover and fix common mistakes in SQL statements, such as exploding joins or full table scans.

To view Query profile, 
1. Go to Query History
2. Click on the name of query and a query details panel appears on the right side of the screen
    <img src="/imagery/dwh_query_profile.jpeg" alt="Query Profile" width="1000" height="auto">
3. Click on 'See query profile' to view the execution details such as 
    * Time spent: Aggregated time spent for each operation. The task’s total time is also provided.
    * Rows: The number and size of the rows affected by each of the query’s operators.
    * Peak memory: The peak memory each of the query’s operators consumed.
    <img src="/imagery/dwh_query_profile_detail.jpeg" alt="Query Execution detail" width="1000" height="auto">

Find more details about Query profile [here](https://learn.microsoft.com/en-us/azure/databricks/sql/user/queries/query-profile)


## Query filters

A query filter lets you interactively reduce the amount of data shown in a visualization. Query filters are similar to query parameter but with a few key differences. A query filter limits data after the query has been executed. This makes filters ideal for smaller datasets and environments where query executions are time-consuming, rate-limited, or costly.
    <img src="/imagery/dwh_query_filters.jpeg" alt="Query Filters" width="1000" height="auto">

Click on 'Add Filters' to add more filters to your data.
    <img src="/imagery/dwh_query_filter_multiple.jpeg.jpeg" alt="Multiple Filters" width="1000" height="auto">

While previous query filters operated client-side only, these updated filters work dynamically on either client- or server-side to optimize performance.

#### Limitations
* The dropdown selector for query filters is limited to 64k unique values. If a user wishes to filter in situations where there are more than 64k unique filter values, it is recommended to use a Text parameter instead.
* Filters can only be applied to columns returned by a query, not all columns of a referenced table.

<br>

## Query Parameters

Query parameters allow you to make the queries more dynamic and flexible by inserting variable values at runtime. Instead of hard-coding specific values into your queries, you can define parameters to filter data or modify output based on user input. This approach improves query reuse, enhances security by preventing SQL injection, and enables more efficient handling of diverse data scenarios.

Type a colon (:) followed by a parameter name to insert parameters into your SQL queries, such as :parameter_name. When a named parameter marker is included in a query, a widget appears in the UI. Use the widget to edit the parameter type and name.
    <img src="/imagery/dwh_parameterisation.jpeg" alt="Query Parameterisation" width="1000" height="auto">

Pass multiple parameters in a single query.
    <img src="/imagery/dwh_parameterisation_multiple.jpeg" alt="Query Multiple Parameters" width="1000" height="auto">
You can drag and drop parameter widgets to your desired position.

Another way to pass parameters in the queries is:
    <img src="/imagery/dwh_param_text.jpeg" alt="Text Parameters" width="1000" height="auto">


## Query snippets

You can also create a query snippet that contains an insertion point with placeholder text that a user can replace at runtime. Query snippets are segments of queries that you can share and trigger using auto complete. Use query snippets for:
* Frequent JOIN statements
* Complicated clauses like WITH or CASE.
* Conditional formatting

#### Create a Snippet
1. Go to Settings -> Developer tab -> SQL query snippets -> SQL Editor -> Create query snippets
    <img src="/imagery/dwh_settings.jpeg" alt="Go to Settings" width="1000" height="auto">

    <img src="/imagery/dwh_snippets_manage.jpeg" alt="Manage Snippets" width="1000" height="auto">
   
2. In the 'Replace' field, enter the snippet name that you will you to reference snippet in the query
3. In the 'Snippet' text box, enter the snippet and click 'Create'

#### Use a query snippet in the query
In your SQL Editor, type the SQL Query followed by the 'Snippet name' and click Run



## Query Caching


Caching is an essential technique for improving the performance of data warehouse systems by avoiding the need to recompute or fetch the same data multiple times. In Databricks SQL, caching can significantly speed up query execution and minimize warehouse usage, resulting in lower costs and more efficient resource utilization. Each caching layer improves query performance, minimizes cluster usage, and optimizes resource utilization for a seamless data warehouse experience.

Caching provides numerous advantages in data warehouses, including:

* **Speed**: By storing query results or frequently accessed data in memory or other fast storage mediums, caching can dramatically reduce query execution times. This storage is particularly beneficial for repetitive queries, as the system can quickly retrieve the cached results instead of recomputing them.<BR>
* **Reduced cluster usage**: Caching minimizes the need for additional compute resources by reusing previously computed results. This reduces the overall warehouse uptime and the demand for additional compute clusters, leading to cost savings and better resource allocation.

#### Types of Query Caches in Databricks SQL
1. **Databricks SQL UI cache**: Per user caching of all query and dashboard results in the Databricks SQL UI. When users first open a dashboard or SQL query, the Databricks SQL UI cache displays the most recent query result, including the results from scheduled executions.

The Databricks SQL UI cache has at most a 7-day life cycle. The cache is located within your Azure Databricks filesystem in your account. You can delete query results by re-running the query that you no longer want to be stored. Once re-run, the old query results are removed from cache. Additionally, the cache is invalidated once the underlying tables have been updated.

2. **Result cache**: Per cluster caching of query results for all queries through SQL warehouses. Result caching includes both local and remote result caches, which work together to improve query performance by storing query results in memory or remote storage mediums.

    1. *Local cache*: The local cache is an in-memory cache that stores query results for the cluster’s lifetime or until the cache is full, whichever comes first. This cache is useful for speeding up repetitive queries, eliminating the need to recompute the same results. However, once the cluster is stopped or restarted, the cache is cleaned and all query results are removed.
    2. *Remote result cache*: The remote result cache is a serverless-only cache system that retains query results by persisting them as workspace system data. As a result, this cache is not invalidated by the stopping or restarting of a SQL warehouse. Remote result cache addresses a common pain point in caching query results in-memory, which only remains available as long as the compute resources are running. The remote cache is a persistent shared cache across all warehouses in a Databricks workspace.
   
Accessing the remote result cache requires a running warehouse. When processing a query, a cluster first looks in its local cache and then looks in the remote result cache if necessary. Only if the query result isn’t cached in either cache is the query executed. Both the local and the remote caches have a life cycle of 24 hours, which starts at cache entry. The remote result cache persists through the stopping or restarting of a SQL warehouse. Both caches are invalidated when the underlying tables are updated.

Remote result cache is available for queries using ODBC / JDBC clients and SQL Statement API.

To disable query result caching, you can run SET use_cached_result = false in the SQL editor.

3. **Disk cache**: Local SSD caching for data read from data storage for queries through SQL warehouses. The disk cache is designed to enhance query performance by storing data on disk, allowing for accelerated data reads. Data is automatically cached when files are fetched, utilizing a fast intermediate format. By storing copies of the files on the local storage attached to compute nodes, the disk cache ensures the data is located closer to the workers, resulting in improved query performance. See Optimize performance with caching on Azure Databricks.


## Query optimisation with constraints

Primary key constraints, which capture relationships between fields in tables, can help users and tools understand relationships in your data. You can use primary keys with the RELY option to optimize some common types of queries.

##### Create table with Primary Key constraint 
```
CREATE TABLE customer (
  c_customer_sk int,
  PRIMARY KEY (c_customer_sk)
  ...
  )
``` 
The primary key constraint specifies that each customer ID value should be unique in the table. Azure Databricks does not enforce key constraints. They can be validated through your existing data pipeline or ETL.

When you know that a primary key constraint is valid, you can enable optimizations based on the constraint by specifying it with the RELY option. See ADD CONSTRAINT clause for the complete syntax.

The RELY option allows Azure Databricks to exploit the constraint to rewrite queries. The following optimizations can only be performed if the RELY option is specified in an ADD CONSTRAINT clause or ALTER TABLE statement.

```
ALTER TABLE
  customer DROP PRIMARY KEY;
ALTER TABLE
  customer
ADD
  PRIMARY KEY (c_customer_sk) RELY;
```

For optimised query performance, run the ANALYZE command on tables for query planning to get smarter. Query patterns change over time, and OPTIMIZE may need help optimally running workloads.
