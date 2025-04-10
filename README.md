# Data Security Package

The Coalesce Data Security Package includes:

* [Dynamic Masking View](dynamic-masking-view)

---

### Dynamic Masking View

The Coalesce Dynamic Data Masking View node type allows you to create a view with masking policies applied to a column within a table or view.

(Dynamic Data Masking)[https://docs.snowflake.com/en/user-guide/security-column-ddm-use] is a Column-level Security feature that uses masking policies to selectively mask data at query time.

Depending on the masking policy conditions, the SQL execution context, and role hierarchy, Snowflake query operators may see the plain-text value, a partially masked value, or a fully masked value.
This node type offers to apply (column-level data masking)[https://docs.snowflake.com/user-guide/security-column-intro] and (row-level access policy)[https://docs.snowflake.com/en/user-guide/security-row-intro] to the target view.

### Prerequisites for Dynamic Masking View

* Create a snowflake masking policy for column-level security and row level access policy for row-level security.This is used in the node type to create a masked view.

Snowflake supports masking policies as a schema-level object to protect sensitive data from unauthorized access while allowing authorized users to access sensitive data at query runtime.

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
| **Snowflake Masking Policy**| Name of snowflake masking policy to mask columns of available column patterns |
| **Override Masking columns**| Toggle: True/False<br/> Provides option to enable masking for specific column specified config |
| **Snowflake masking Column Name**| Enabled when Override Masking columns option is true.The column on which data masking to be applied |
| **Snowflake masking policy Name**| Name of the snowflake masking policy.Different masking policy for different columns is possible |
| **Enable row level Masking** | Toggle: True/False<br/> Provides option to enable row level access restriction |
| **Row access policy name**| Name of snowflake row access policy |
| **Row access column name**| The column name(s) on whose availability in the table ,row level access is provided|
