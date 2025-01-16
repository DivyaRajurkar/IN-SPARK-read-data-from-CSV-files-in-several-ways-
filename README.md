# IN-SPARK-read-data-from-CSV-files-in-several-ways-
In Spark, you can read data from CSV files in several ways using the **`csv` method** in the **`DataFrameReader`**. Here are the main approaches:

---

### 1. **Using `spark.read.csv()`**  
This is the most common method to read CSV files in Spark.

```python
df = spark.read.csv("path/to/file.csv")
```

- **Default behavior**: The first row is not treated as a header, and all columns are read as strings.

---

### 2. **With Options for Better Control**  
You can specify options like `header`, `inferSchema`, and `delimiter`.

```python
df = spark.read.option("header", True) \
               .option("inferSchema", True) \
               .option("delimiter", ",") \
               .csv("path/to/file.csv")
```

- **`header`**: Whether the first row should be used as column names. (`True` or `False`)
- **`inferSchema`**: Whether to automatically infer data types. (`True` or `False`)
- **`delimiter`**: Specifies the delimiter (e.g., `,`, `;`, or `|`).

---

### 3. **Using `format` Method**  
Instead of directly calling `csv`, you can specify the format explicitly.

```python
df = spark.read.format("csv") \
               .option("header", True) \
               .option("inferSchema", True) \
               .load("path/to/file.csv")
```

---

### 4. **Reading Multiple CSV Files**  
You can read multiple files by providing a path pattern or a list of paths.

- **Path pattern**:

```python
df = spark.read.csv("path/to/folder/*.csv")
```

- **List of paths**:

```python
df = spark.read.csv(["path/to/file1.csv", "path/to/file2.csv"])
```

---

### 5. **Using Schema Definition**  
Define a schema explicitly instead of inferring it.

```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

schema = StructType([
    StructField("Name", StringType(), True),
    StructField("Age", IntegerType(), True),
    StructField("City", StringType(), True)
])

df = spark.read.format("csv") \
               .option("header", True) \
               .schema(schema) \
               .load("path/to/file.csv")
```

---

### 6. **Reading from Compressed Files**  
Spark supports reading compressed CSV files directly (e.g., `.gz`, `.zip`).

```python
df = spark.read.csv("path/to/file.csv.gz")
```

---

### 7. **Using SQL**  
You can create a temporary view and use SQL queries to read the data.

```python
df = spark.read.csv("path/to/file.csv", header=True, inferSchema=True)
df.createOrReplaceTempView("csv_table")
result = spark.sql("SELECT * FROM csv_table WHERE Age > 25")
```

---

### 8. **Reading CSV from HDFS/S3/Azure Blob**  
Spark can directly read from distributed storage systems.

- **HDFS**:

```python
df = spark.read.csv("hdfs://namenode:9000/path/to/file.csv")
```

- **Amazon S3**:

```python
df = spark.read.csv("s3://bucket-name/path/to/file.csv")
```

- **Azure Blob**:

```python
df = spark.read.csv("wasbs://container@account.blob.core.windows.net/path/to/file.csv")
```

---

### 9. **Reading CSV with Advanced Options**
- Ignore corrupt records:

```python
df = spark.read.option("mode", "DROPMALFORMED").csv("path/to/file.csv")
```

- Treat null values:

```python
df = spark.read.option("nullValue", "NA").csv("path/to/file.csv")
```

---

These methods make Spark flexible in reading CSV data from different sources and with varying levels of customization! Let me know if you need further details on any specific method. 😊
