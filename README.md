# 🧠 Delta Management Toolkit

**Simplify Delta Lake operations with SQL-like utilities**

Coming from a SQL Server background, working with **Delta Lake** can feel disorienting at first — you can’t just `ALTER TABLE`, `DROP COLUMN`, or `DELETE FROM` like before.  
This toolkit provides a collection of Databricks/Fabric notebooks and helper functions that make Delta management **simple, safe, and familiar** again.

---

## 📂 Repository Contents

| Notebook | Description |
|-----------|-------------|
| `DeltaManagement_Column_Drop` | Safely drop one or more columns using Delta’s column mapping mode |
| `DeltaManagement_Column_Rename` | Rename columns using a `{old_name: new_name}` dictionary |
| `DeltaManagement_Column_Types` | Modify column data types similar to `ALTER TABLE ... ALTER COLUMN` |
| `DeltaManagement_Rows_Delete` | Delete rows with a SQL-like `WHERE` condition (with preview) |
| `DeltaManagement_AutoCompaction` | Enable automatic file compaction and optimized writes |
| `Maintenance - Vacuum` | Clean up old Delta transaction log files and manage storage |

---

## 🔧 Core DDL & DML Utility Functions

These helper functions emulate SQL Server–style schema and data operations for Delta tables.

### 1. Drop Columns
```python
drop_columns(table_name, cols)
```

### 2. Rename Columns

```python
update_table_properties_and_rename_columns(table_name, rename_dict)
```
Renames columns using a {old_name: new_name} dictionary.
Requires column mapping mode ('delta.columnMapping.mode' = 'name').

### 3. Alter Column Types

```python
alter_column_types(table_name, type_change_dict)
```
Changes column types directly using SQL syntax — similar to:
```sql
ALTER TABLE my_table ALTER COLUMN column_name TYPE new_type;
```

### 4. Delete rows
```python
delete_rows(table_name, condition)
```
Deletes rows matching a given condition (for example, Username LIKE '%pikabea%').
Previews the rows before executing the delete for safety.

### Required Table Properties

Before running DDL operations (drop, rename, alter), set these properties:
```sql
ALTER TABLE my_table SET TBLPROPERTIES (
    'delta.minReaderVersion' = '2',
    'delta.minWriterVersion' = '5',
    'delta.columnMapping.mode' = 'name'
);
```

| Property | Purpose |
|-----------|-------------|
| `'delta.columnMapping.mode' = 'name'` | Enables column tracking by name (required for renames/drops) |
| `'delta.minReaderVersion' = '2'` | Ensures readers are compatible (Delta v2+) |
| `'delta.minWriterVersion' = '5'` | Ensures writers are compatible (Delta v5+) |

These properties make schema evolution safe and predictable — similar to SQL Server, but built for a distributed environment.

---

## 🧹 Maintenance & Performance Utilities

In the Lakehouse world, performance tuning replaces traditional database maintenance.
These notebooks help keep your Delta tables optimized and cost-efficient.

### 1. Auto-Compaction & Optimized Writes
```python
spark.conf.set("spark.databricks.delta.autoCompact.enabled", "true")
spark.conf.set("spark.databricks.delta.optimizeWrite.enabled", "true")
```
Automatically merges small files and optimizes Parquet layout for faster queries.

### 2. Vacuum
```python
spark.sql("VACUUM my_table RETAIN 168 HOURS")
```
Removes unreferenced files and controls storage costs (defined retention: 7 days)

### 3. Adaptive Target File Size (Microsoft Fabric / Synapse / Databricks)
```python
spark.conf.set("spark.microsoft.delta.targetFileSize.adaptive.enabled", True)
```

---
## 💡 Takeaway
Use this toolkit as your daily Delta management companion:

- 🧰 DDL/DML helpers — make schema and data operations intuitive again
- ⚙️ Maintenance utilities — keep Delta tables efficient and clean
- 🚀 Ideal for Microsoft Fabric, Azure Databricks, or open-source Delta Lake environments

Together, they bring structure, reliability, and performance to your Lakehouse —
just like a well-managed SQL Server, but built for modern distributed data engineering.

---
## 📘 References & Resources

- [Mastering Spark: The Art and Science of Table Compaction | Miles Cole](https://milescole.dev/data-engineering/2025/02/26/The-Art-and-Science-of-Table-Compaction.html)
- [Adaptive Target File Size Management in Fabric Spark | Microsoft Fabric Blog](https://blog.fabric.microsoft.com/en-us/blog/adaptive-target-file-size-management-in-fabric-spark?ft=All)

