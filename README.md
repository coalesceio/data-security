# Data Security Package

The Coalesce Data Security Package includes:

* [Dynamic Masking View](#dynamic-masking-view)
* [Code](#code)

---

## Dynamic Masking View

The Coalesce Dynamic Data Masking View node type allows you to create a view with masking policies applied to a column within a table or view.

[Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-use) is a Column-level Security feature that uses masking policies to selectively mask data at query time.

Depending on the masking policy conditions, the SQL execution context, and role hierarchy, Snowflake query operators may see the plain-text value, a partially masked value, or a fully masked value.
This node type offers to apply [column-level data masking](https://docs.snowflake.com/user-guide/security-column-intro) and [row-level access policy](https://docs.snowflake.com/en/user-guide/security-row-intro) to the target view.

### Prerequisites for Dynamic Masking View

* Create a snowflake masking policy for column-level security and row level access policy for row-level security.This is used in the node type to create a masked view.

Snowflake supports masking policies as a schema-level object to protect sensitive data from unauthorized access while allowing authorized users to access sensitive data at query runtime.

### Limitations of Dynamic Masking View

* A column can only be associated with one masking policy at a time.
* The input and output data types in a masking policy must match; you can't define a policy to target a timestamp and return a string.
* Once a materialized view is created from a table, you cannot set masking policies on any of its columns
* Cannot apply a masking policy to a table column if a materialized view is already created from the underlying table.
* A given table or view column can be specified in either a row access policy signature or a masking policy signature.

#### Examples

![image](https://github.com/user-attachments/assets/21b89bd5-60fb-4dfb-b1ed-c6e1eb7a6cb1)

![image](https://github.com/user-attachments/assets/b77cfcca-e465-429d-88c5-d6bcc0912890)

### Dynamic Masking View Node Configuration

The Work node type has four configuration groups:

* [Node Properties](#node-properties)
* [Options](#options)

#### Node Properties

| **Property** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the target view will be created |
| **Node Type** | Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/> If FALSE the node will not be deployed or will be dropped during redeployment |

#### Options

| **Options** | **Description** |
|---------|-------------|
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible. DISTINCT data is chosen for processing<br/>**False**: Group by All is visible |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible. Data is grouped by all columns for processing<br/>**False**: DISTINCT is visible |
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join |
| **Enable Column Masking** | Toggle: True/False<br/> Provides option to enable column masking |
| **Coalesce Storage Location of Data Masking Policy**| Enabled when Column Masking is true.Storage location in Coalesce where the Masking policy resides |
| **Snowflake Masking Policy**| Name of snowflake masking policy to mask columns of available column patterns |
| **Override Masking columns**| Toggle: True/False<br/> Provides option to enable masking for specific column specified config |
| **Snowflake masking Column Name**| Enabled when Override Masking columns option is true.The column on which data masking to be applied |
| **Snowflake masking policy Name**| Name of the snowflake masking policy.Different masking policy for different columns is possible |
| **Enable row level security** | Toggle: True/False<br/> Provides option to enable row level access restriction |
| **Coalesce Storage Location of row access policy**| Enabled when row level security is true.Storage location in Coalesce where the Row access policy resides |
| **Row access policy name**| Name of snowflake row access policy |
| **Row access column name**| The column name(s) on whose availability in the table ,row level access is enabled|

### Enable Column Masking

![image](https://github.com/user-attachments/assets/5b4d6d43-acee-40b3-87bb-e90af053f7dd)

### Enable row level security 

![image](https://github.com/user-attachments/assets/d4886da5-9f92-4c76-b11f-831d257104d7)

### Dynamic Masking View Deployment

#### Dynamic Masking View Initial Deployment

When deployed for the first time into an environment the View node will execute the Create View stage.

| **Stage** | **Description** |
|-----------|----------------|
| **Create View** | This will execute a CREATE OR REPLACE statement and create a View in the target environment |

#### Dynamic Masking View Redeployment

The subsequent deployment of View node with changes in view definition, adding table description, adding secure option or renaming view results in deleting the existing view and recreating the view.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Delete View** | Removes existing view |
| **Create View** | Creates new view with updated definition |

#### Dynamic Masking View Undeployment

If a View Node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the View in the target environment will be dropped.

This is executed in the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Delete View** | Removes the view from the environment |

## Code

* [Node definition](https://github.com/coalesceio/data-security/blob/main/nodeTypes/DynamicMaskingView-455/definition.yml)
* [Create Template](https://github.com/coalesceio/data-security/blob/main/nodeTypes/DynamicMaskingView-455/create.sql.j2)

[Macros](https://github.com/coalesceio/data-security/blob/main/macros/macro-1.yml)
