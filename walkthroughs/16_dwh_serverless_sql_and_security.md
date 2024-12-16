# Serverless SQL in Databricks

Serverless SQL warehouses in Databricks are designed to provide on-demand, scalable compute resources for running SQL queries without the need to manage infrastructure. Here are some key points to understand:

## What is Serverless Compute?
Serverless compute allows you to run workloads without provisioning a cluster. Instead, Databricks automatically allocates and manages the necessary compute resources. This enables you to focus on writing code and analysing data without worrying about cluster management or resource utilisation.


## Benefits of Serverless SQL Warehouses

* **Instant Compute:** Rapid start-up and scaling times for serverless compute resources minimise idle time and ensure you only pay for the compute you use.
* **Managed Resources:** Cloud resources are managed by Databricks, reducing management overhead and providing instant compute to enhance user productivity.
* **Security and Reliability:** Capacity handling, security, patching, and upgrades are managed automatically, so you can worry less about reliability, security policies, and capacity shortages.
* **How Serverless SQL Warehouses Work:** Serverless SQL warehouses use private connectivity between the Databricks control plane and the serverless compute plane in nearly all cases. This ensures secure and efficient data processing.


## Use Cases:
* **Interactive Queries:** Ideal for running SQL commands on data objects in the SQL editor or interactive notebooks.
* **Batch Processing:** Suitable for batch processing tasks where you need to run large volumes of data efficiently.
* **Enabling Serverless SQL Warehouses:** Serverless SQL warehouses are enabled by default in Databricks. However, you may need to check that your workspace meets the necessary requirements and make any required changes.

<br>

# Security in Databricks

## SQL Warehouse Settings and Access Controls

Databricks recommends retaining the default settings for all workspace-level configurations for SQL warehouses. These settings assume that workspace admins are responsible for creating and configuring all SQL warehouses and that you use Unity Catalog for data governance.

Workspace administrators can configure the following permissions for an Azure Databricks workspace:

### Revoke all access to SQL warehouses.

You can revoke access to SQL warehouses for a user, service principal, or group by unassigning the Databricks SQL access entitlement.

### Grant the ability to create SQL warehouses.

You can grant SQL warehouse creation privileges to a user, service principal, or group by assigning the Allow unrestricted cluster creation entitlement. 

### Configure default parameters that control the SQL warehouse compute environment.

Click your username in the top bar of the workspace and select Settings -> Compute -> Manage next to SQL Warehouse 
In the SQL Configuration Parameters textbox, specify one key-value pair per line. Separate the name of the parameter from its value using a space. 
For example, to enable ANSI_MODE, type "ANSI_MODE true" and click Save

### Configure data access policies for SQL warehouses.

Databricks recommends managing data access policies using Unity Catalog.

### Transfer ownership of a SQL warehouse

The user you transfer ownership of a SQL warehouse to must have the Allow unrestricted cluster creation entitlement.

1. As a workspace admin, log in to your Azure Databricks workspace.
2. Click SQL Warehouses in the sidebar.
3. In a warehouse row, click the Kebab menu kebab menu at the far right and select Permissions. The SQL warehouse permissions display.
4. Click on the gear icon at the top right and click Assign new owner.
5. Select the user to assign ownership to. Service principals and groups cannot be assigned ownership of a SQL warehouse.
6. Click Confirm.
