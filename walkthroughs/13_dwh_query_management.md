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

   For more detailed information about the query’s performance, including its execution plan, click [View Query Profile](https://docs.databricks.com/en/sql/user/queries/query-profile.html) near the bottom of the page
<br>

1. To cancel a running query, select the Query and scroll down to click Cancel Query button at the bottom right of screen.
    <img src="/imagery/dwh_cancel_running_query.jpeg" alt="Cancel running query" width="1000" height="auto">
  

## Query Caching



## Query filters



## Query parameters



## Query snippets



## Query profile



## Query optimisation with constraints


