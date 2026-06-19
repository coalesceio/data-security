# Data Security Package

The Coalesce Data Security Package helps you apply column-level data masking and row-level access policies to Snowflake views.

The package includes:

* [Dynamic Masking View](#dynamic-masking-view)
* [Masking Enabled Materialized View](#masking-enabled-materialized-view)
* [Code](#code)

---

## Dynamic Masking View

The Coalesce Dynamic Data Masking View Node type allows you to create a view with masking policies applied to a column within a table or view.

[Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-use) is a Column-level Security feature that uses masking policies to selectively mask data at query time.

Depending on the masking policy conditions, the SQL execution context, and role hierarchy, Snowflake query operators may see the plain-text value, a partially masked value, or a fully masked value.
This Node type applies [column-level data masking](https://docs.snowflake.com/user-guide/security-column-intro) and [row-level access policy](https://docs.snowflake.com/en/user-guide/security-row-intro) to the target view.

### Prerequisites for Dynamic Masking View

* Create a Snowflake masking policy for column-level security and a row-level access policy for row-level security. This is used in the Node type to create a masked view.

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

The Dynamic Masking View Node type has 4 configuration groups:

* [Node Properties](#node-properties)
* [Options](#options)

#### Node Properties

| **Property** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the target view will be created |
| **Node Type** | Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the Node will be deployed or redeployed when changes are detected<br/> If FALSE the Node will not be deployed or will be dropped during redeployment |

#### Options

| **Options** | **Description** |
|---------|-------------|
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible. DISTINCT data is chosen for processing<br/>**False**: Group by All is visible |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible. Data is grouped by all columns for processing<br/>**False**: DISTINCT is visible |
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join |
| **Enable Column Masking** | Toggle: True/False<br/> Provides option to enable column masking |
| **Coalesce Storage Location of Data Masking Policy**| Enabled when Column Masking is true. Storage location in Coalesce where the Masking policy resides |
| **Snowflake Masking Policy**| Name of Snowflake masking policy to mask columns of available column patterns |
| **Override Masking columns**| Toggle: True/False<br/> Provides option to enable masking for specific column specified config |
| **Snowflake masking Column Name**| Enabled when Override Masking columns option is true. The column on which data masking is applied |
| **Snowflake masking policy Name**| Name of the Snowflake masking policy. Different masking policies for different columns are supported |
| **Enable row level security** | Toggle: True/False<br/> Provides option to enable row level access restriction |
| **Coalesce Storage Location of row access policy**| Enabled when row level security is true. Storage location in Coalesce where the Row access policy resides |
| **Row access policy name**| Name of Snowflake row access policy |
| **Row access column name**| The column name or names that control row-level access in the table |

### Enable Column Masking

![image](https://github.com/user-attachments/assets/5b4d6d43-acee-40b3-87bb-e90af053f7dd)

### Enable Row Level Security

![image](https://github.com/user-attachments/assets/d4886da5-9f92-4c76-b11f-831d257104d7)

### Dynamic Masking View Deployment

#### Dynamic Masking View Initial Deployment

When deployed for the first time into an environment, the View Node executes the Create View stage.

| **Stage** | **Description** |
|-----------|----------------|
| **Create View** | This will execute a CREATE OR REPLACE statement and create a View in the target environment |

#### Dynamic Masking View Redeployment

Subsequent deployment of the View Node with changes to the view definition, table description, secure option, or view name deletes the existing view and recreates it.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Delete View** | Removes existing view |
| **Create View** | Creates new view with updated definition |

#### Dynamic Masking View Undeployment

If a View Node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher-level Environment, then the View in the target Environment is dropped.

This executes the following stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Delete View** | Removes the view from the environment |

---


## Masking Enabled Materialized View

The Coalesce Masking Enabled Materialized View Node type allows you to create a Snowflake Materialized View with masking policies and row access policies applied to sensitive data.

These features help protect sensitive information while allowing authorized users to access data based on their assigned roles and policy conditions.

This Node Type applies
* Column-level data masking using Snowflake Masking Policies.
* Row-level security using Snowflake Row Access Policies.
* Materialized View clustering and secure view options.

## Prerequisites for Masking Enabled Materialized View

Before using this Node type:

* Create the required Snowflake Masking Policies.
* Create the required Snowflake Row Access Policies.
* Ensure the policies are deployed in a Snowflake schema accessible by the target environment.
* Configure the corresponding Storage Locations in Coalesce.

Snowflake masking policies are schema-level objects that protect sensitive data from unauthorized access while still allowing authorized users to access data at query runtime.

## Limitations of Masking Enabled Materialized View

* A column can only have one masking policy attached at a time.
* The input and output data types in a masking policy must match.
* A given column can participate in either a masking policy signature or a row access policy signature.
* Materialized Views only support deterministic expressions.
* Changes to source columns, transformations, joins, or materialized view definitions may require the Materialized View to be recreated.
* Snowflake limitations applicable to Materialized Views also apply to this Node type.

## Masking Enabled Materialized View Node Configuration

The Masking Enabled Materialized View Node type contains the following configuration groups:

* [Node Properties](#node-properties)
* [Options](#options)


#### Node Properties

| **Property** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the target view will be created |
| **Node Type** | Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the Node will be deployed or redeployed when changes are detected<br/> If FALSE the Node will not be deployed or will be dropped during redeployment |

#### Options

<img width="456" height="581" alt="image" src="https://github.com/user-attachments/assets/07f438ff-ff19-43b9-a802-26e252d659d3" />


| **Options** | **Description** |
|---------|-------------|
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible. DISTINCT data is chosen for processing<br/>**False**: Group by All is visible |
| **Cluster key** | True/False to determine whether Materialized view is to be clustered or not<br/>- **True**: Allows you to specify the column based on which clustering is to be done<br/>  -**Allow Expressions Cluster Key**: True allows to add an expression to the specified cluster key<br/>- **False**: No clustering done |
| **Secure** | True / False Toggle to determine whether Materialized view to be created in a secured mode<br/>- **True**: Materialized view created in a secured mode<br/>- **False**: No additional secure option added during Materialized view creation |
| **Enable Column Masking** | Toggle: True/False<br/> Provides option to enable column masking |
| **Coalesce Storage Location of Data Masking Policy**| Enabled when Column Masking is true. Storage location in Coalesce where the Masking policy resides |
| **Snowflake Masking Policy**| Name of Snowflake masking policy to mask columns of available column patterns |
| **Override Masking columns**| Toggle: True/False<br/> Provides option to enable masking for specific column specified config |
| **Snowflake masking Column Name**| Enabled when Override Masking columns option is true. The column on which data masking is applied |
| **Snowflake masking policy Name**| Name of the Snowflake masking policy. Different masking policies for different columns are supported |
| **Enable row level security** | Toggle: True/False<br/> Provides option to enable row level access restriction |
| **Coalesce Storage Location of row access policy**| Enabled when row level security is true. Storage location in Coalesce where the Row access policy resides |
| **Row access policy name**| Name of Snowflake row access policy |
| **Row access column name**| The column name or names that control row-level access in the table |

## Column Masking Behavior

### Default Masking Policy

When **Enable Column Masking** is enabled and **Override Masking Columns** is disabled, the selected Snowflake Masking Policy is automatically applied to supported column patterns.

Supported column patterns include:

```text
PHONE
ADDRESS
NAME
ACCOUNT
EMAIL
INCOME
```

Any column containing one of these patterns in its name will automatically receive the selected masking policy.


### Override Masking Columns

When **Override Masking Columns** is enabled, specific columns can be assigned their own masking policies.

This allows different columns within the same Materialized View to use different masking policies.

Example:

| Column | Policy      |
| ------ | ----------- |
| SSN    | SSN_MASK    |
| SALARY | SALARY_MASK |
| EMAIL  | EMAIL_MASK  |

## Row Level Security Behavior

When **Enable Row Level Security** is enabled, a Snowflake Row Access Policy is attached to the Materialized View.

The selected columns are passed to the Row Access Policy signature.

> **Note:** The number and order of columns selected in Coalesce must match the columns defined in the Snowflake Row Access Policy signature. If the policy is created using one column, select one column in Coalesce. If the policy is created using multiple columns, select the corresponding columns in the same order.

## Masking Enabled Materialized View Deployment

### Initial Deployment

When deployed for the first time, the Node executes a CREATE OR REPLACE statement to create the Materialized View.

The following stage is executed:

| Stage                                    | Description                                                                   |
| ---------------------------------------- | ----------------------------------------------------------------------------- |
| Create Masking Enabled Materialized View | Creates the Materialized View with configured masking and row access policies |


## Redeployment

During redeployment, the Node intelligently determines whether a CREATE or ALTER operation is required.

### CREATE Operations

The Materialized View is recreated when:

* Source columns change.
* Column transformations change.
* Source joins change.
* Materialization type changes.
* DISTINCT configuration changes.

The following stages may be executed:

| Stage                                    | Description                            |
| ---------------------------------------- | -------------------------------------- |
| Drop Materialized View                   | Removes the existing Materialized View |
| Create Masking Enabled Materialized View | Creates the updated Materialized View  |


### ALTER Operations

The following changes are handled through ALTER statements without recreating the Materialized View:

#### Materialized View Properties

| Stage                                   | Description                           |
| --------------------------------------- | ------------------------------------- |
| Alter Materialized View - Rename        | Renames the Materialized View         |
| Alter Materialized View - Comment       | Updates the Materialized View comment |
| Alter Materialized View - Secure Option | Sets or removes Secure Mode           |
| Recluster Materialized View             | Updates clustering configuration      |
| Resume Recluster Materialized View      | Resumes automatic reclustering        |


#### Column Masking

| Stage                  | Description                                                        |
| ---------------------- | ------------------------------------------------------------------ |
| Set Masking Policy     | Applies a masking policy to a column                               |
| Replace Masking Policy | Replaces an existing masking policy                                |
| Unset Masking Policy   | Removes a masking policy from a column                             |
| Apply Default Policy   | Applies the selected node-level masking policy                     |
| Remove Override Policy | Removes an override masking policy and restores node-level masking |


#### Row Level Security

| Stage                     | Description                                           |
| ------------------------- | ----------------------------------------------------- |
| Add Row Access Policy     | Attaches a Row Access Policy                          |
| Drop Row Access Policy    | Removes a Row Access Policy                           |
| Replace Row Access Policy | Replaces an existing Row Access Policy with a new one |


## Undeployment

If the Node is removed from a Workspace and the changes are deployed, the Materialized View is dropped from the target environment.

The following stage is executed:

| Stage                  | Description                                        |
| ---------------------- | -------------------------------------------------- |
| Drop Materialized View | Removes the Materialized View from the environment |

## Code

#### Dynamic Masking View
* [Node definition](https://github.com/coalesceio/data-security/blob/main/nodeTypes/DynamicMaskingView-455/definition.yml)
* [Create Template](https://github.com/coalesceio/data-security/blob/main/nodeTypes/DynamicMaskingView-455/create.sql.j2)

#### Masking Enabled Materialized View
* [Node definition](https://github.com/coalesceio/data-security/blob/main/nodeTypes/MaskingEnabledMaterializedView-178/definition.yml)
* [Create Template](https://github.com/coalesceio/data-security/blob/main/nodeTypes/MaskingEnabledMaterializedView-178/create.sql.j2)


[Macros](https://github.com/coalesceio/data-security/blob/main/macros/macro-1.yml)
