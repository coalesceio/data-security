# Data Security Package

The Coalesce Data Security Package includes:

* [Dynamic Masking View](dynamic-masking-view)

---

### Dynamic Masking View

The Coalesce Dynamic Data Masking View node type allows you to create a view with masking policies applied to a column within a table or view.(Dynamic Data Masking)[] is a Column-level Security feature that uses masking policies to selectively mask data at query time
Depending on the masking policy conditions, the SQL execution context, and role hierarchy, Snowflake query operators may see the plain-text value, a partially masked value, or a fully masked value.
This node type offers to apply (column-level data masking)[] and (row-level access policy)[] to the target view.

### Prerequisites for Dynamic Masking View


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
