# Working with Spark DataFrames in PySpark

## 1. Importing SparkSession

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PySpark DataFrame Basics") \
    .getOrCreate()
```
## 2. Read json file
```python
df_json = spark.read.json("path/to/file.json")
df_json.show()
```
---
## Sample CSV File for Practice

Let's use this sample CSV file that contains multiple data types:

| employee_id | name     | age | salary  | department | dob        | join_date | active | bonus  |
|-------------|----------|-----|---------|------------|------------|-----------|--------|--------|
| 101         | Alice    | 29  | 75000.5 | HR         | 1994-06-12 | 2020-08-01| true   |        |
| 102         | Bob      | 35  | 88000.0 | IT         | 1988-03-09 | 2019-03-15| true   | 5000.0 |
| 103         | Charlie  |     | 64000.0 | Finance    | 1992-01-20 | 2021-01-10| false  |        |
| 104         | Diana    | 42  | 97000.0 | IT         | 1981-11-03 | 2017-05-25| true   | 8000.0 |
| 105         | Edward   | 31  |         | Marketing  | 1991-07-14 | 2022-10-12| false  |        |

> 💡 You can save this as `employees.csv` and load it in PySpark for practice.

---

## 3. Read csv file
```python
df_csv = spark.read.format('csv').options(header=True, inferSchema= True).load("path/to/employees.csv")
df_csv.display()
```

## 3. Adding or Updating Columns with `.withColumn()`

The `.withColumn()` method lets you add a new column or update an existing one.

### Example: Add a column that doubles the age

```python

df_with_doubled_age = df.withColumn("double_age", col("age") * 2)
df_with_doubled_age.show()
````

---

## 4. Creating or Replacing a Temporary SQL View

PySpark allows you to use SQL syntax on DataFrames by registering them as a temporary table.

### Example:

```python
df.createOrReplaceTempView("employee_table")

spark.sql("SELECT name, salary FROM employee_table WHERE department = 'IT'").show()
```

---

## 5. Basic DataFrame Operations

### Filtering Rows

```python
df.filter(df["salary"] > 80000).show()
```

### Grouping and Aggregating Data

```python
df.groupBy("department").count().show()

df.groupBy("department").agg({"salary": "avg"}).show()
```

### Ordering Data

```python
df.orderBy("age", ascending=False).show()
```

---

## 6. Useful Functions for Aggregation

```python
from pyspark.sql.functions import avg, format_number, countDistinct, stddev

df.select(
    avg("salary").alias("average_salary"),
    format_number(avg("salary"), 2).alias("formatted_avg_salary"),
    countDistinct("department").alias("unique_departments"),
    stddev("salary").alias("salary_stddev")
).show()
```

---

## 7. Handling Missing (Null) Data

PySpark provides the `.na` submodule to handle missing data.

### Drop Rows with Any Nulls:

```python
df.na.drop().show()
```

### Fill Missing Values:

```python
df.na.fill({
    "age": 0,
    "salary": 50000.0,
    "bonus": 0.0
}).show()
```

---

## 8. Working with Date and Timestamp Columns

```python
from pyspark.sql.functions import to_date, to_timestamp, current_date

df_date = df.withColumn("dob_date", to_date("dob", "yyyy-MM-dd"))
df_timestamp = df.withColumn("join_date_time", to_timestamp("join_date", "yyyy-MM-dd"))

df_date.select("name", "dob_date").show()

df_timestamp.select("name", "join_date_time").show()

df.select(current_date().alias("today")).show()
```

---
