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


## Query profile


## Query Caching


## Query optimisation with constraints





Query planning gets smarter by using statistics, but that requires you to know how to run the ANALYZE command. However, fewer than 5% of customers run ANALYZE. And, because tables can have hundreds of columns (or more) and query patterns change over time, you may need help optimally running workloads.

Specifically, you may have these situations:

Data Engineers have to manage “optimization” jobs to maintain statistics
Data Engineers have to determine which tables need to have statistics updated and how often
Data Engineers have to ensure that the key columns are in the first 32
Data Engineers have to potentially rebuild tables if query patterns change or new columns are added
With these new intelligent statistics enabled, statistics are managed in two phases. First, statistics are collected for all new data written with Photon-enabled compute. This is faster and cheaper because the data is read only once (compared to running ANALYZE after ingestions). Then, as statistics become stale (due to UPDATE and DELETE statements), this process runs ANALYZE in the background to ensure the statistics are always fresh.

 

To sign up for the Gated Public Preview of Predictive Optimization for statistics,