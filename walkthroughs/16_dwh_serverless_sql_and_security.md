# Serverless SQL and Security in Databricks

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

* As a workspace admin, log in to your Azure Databricks workspace.
* Click SQL Warehouses in the sidebar.
* In a warehouse row, click the Kebab menu kebab menu at the far right and select Permissions. The SQL warehouse permissions display.
* Click on the gear icon at the top right and click Assign new owner.
* Select the user to assign ownership to. Service principals and groups cannot be assigned ownership of a SQL warehouse.
* Click Confirm.


## Data Access Configurations


Note: If your workspace is enabled for Unity Catalog, you don’t need to perform the steps in this article. Unity Catalog supports SQL warehouses by default.