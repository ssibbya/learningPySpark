# Getting Started with Databricks Free Edition

Databricks is a cloud-based platform that makes working with big data and Apache Spark easy. The **Community Edition** is free and ideal for learning PySpark and exploring the Databricks environment.

---

## 🔐 Step 1: Sign Up / Log In

1. Visit: [https://www.databricks.com/learn/free-edition](https://www.databricks.com/learn/free-edition)
2. Sign up using your **email ID** and set a password.
3. Log in to access your workspace.

> ✅ Tip: You get a free cluster with limited resources, perfect for learning and development.

---

## 🧭 Step 2: Explore the User Interface

Once logged in, familiarize yourself with key sections on the left panel:

### 🔹 Workspace
Create and manage notebooks and folders.

### 🔹 Catalog
Contains data assets including schemas, tables, and volumes.

### 🔹 Compute
Where you start and manage your clusters.

---

## 📁 Step 3: Upload Data Using Volumes (DBFS is Deprecated)

> ⚠️ **Important**: Public **DBFS** (`/FileStore/tables/...`) is deprecated in Community Edition. Use **Volumes** inside the Unity Catalog instead.

### ✅ How to Upload Files Using Volumes:

1. Go to **Catalog** → **Workspace** → **default**
2. Click the **Volumes** tab
3. Click **Create Volume**
4. Name your volume (e.g., `myvolume`)
5. Once created, click on the volume → **Upload** your CSV or JSON file

> Example uploaded file path:
```

/Volumes/workspace/default/myvolume/employees.csv

````

---

## 📊 Step 4: Read the File Using PySpark

Once your file is uploaded, read it in your notebook:

```python
df = spark.read.option("header", True).csv("/Volumes/workspace/default/myvolume/employees.csv")
df.show()
````

> Use `.json()` instead of `.csv()` for JSON files.

---

## ✅ Additional Tips

* Use `.option("inferSchema", True)` to automatically detect data types.
* Make sure your cluster is running before executing any Spark code.
* Use `df.printSchema()` to check your DataFrame structure.

---

## ✅ Summary

* Login to Databricks Free Edition
* Navigate to `Catalog > Workspace > default > Volumes` to upload data
* DBFS is deprecated — always use Volumes
* Use `.load()`, `.csv()`, or `.json()` with correct volume paths

---
