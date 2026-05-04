# 🚀 Azure Data Factory - Dynamic File Ingestion Pipeline

## 📌 Project Overview

This project demonstrates a **dynamic file ingestion pipeline** built using Azure Data Factory (ADF). The pipeline reads multiple files from a source Azure Data Lake Storage (ADLS Gen2) folder and processes them dynamically without hardcoding file names.

---

## 🧠 Problem Statement

In real-world data engineering scenarios:

* Files arrive daily in folders
* File names are unknown beforehand
* Manual pipelines are not scalable

👉 Solution: Build a **dynamic, metadata-driven pipeline**

---

## 🏗️ Architecture

Source (ADLS - raw/sales)
→ Get Metadata (fetch file list)
→ ForEach (loop through files)
→ Copy Activity
→ Target (ADLS - processed/sales)

---

## ⚙️ Pipeline Components

### 1. Get Metadata Activity

* Retrieves list of files using `childItems`
* Dataset points to folder (not file)

### 2. ForEach Activity

* Iterates over each file dynamically
* Input:

```
@activity('Get Metadata1').output.childItems
```

### 3. Copy Activity

* Copies each file from source to sink
* Uses dynamic file name:

```
@item().name
```

---

## 📂 Datasets

### Source Dataset

* Points to:

```
raw/sales/
```

* File name parameterized

### Sink Dataset

* Points to:

```
raw/processed
```

* File name dynamically assigned

### Metadata Dataset

* Used only for folder-level operations

---

## 🔧 Key Features

✔️ Dynamic file processing
✔️ No hardcoding of file names
✔️ Parameterized datasets
✔️ Scalable pipeline design
✔️ Handles multiple files automatically

---

## ❌ Challenges Faced & Fixes

### 1. No files processed (0 bytes issue)

* Cause: Incorrect file path configuration
* Fix: Used `File path in dataset` + dynamic file name

### 2. Overwriting files

* Cause: Static sink file name
* Fix:

```
@item().name
```

### 3. childItems empty

* Cause: Dataset misconfiguration
* Fix: Correct folder path + selected `childItems`

### 4. Wrong file path type

* Cause: Used wildcard / list incorrectly
* Fix: Switched to dataset-based path

---

## 🧪 Sample Output

Processed files:

```
raw/processed
   ElectricCarData_Clean.csv
   ElectricCarData_Norm.csv
   churn-bigml-20.csv
   churn-bigml-80.csv
   orders.csv
```

---

## 💡 Key Learnings

* Difference between folder vs file dataset
* Importance of dynamic expressions in ADF
* Using `@item().name` inside ForEach
* Debugging pipelines with zero-data issues
* Avoiding overwrite problems

---

## 🚀 Future Enhancements

* Load data into Azure SQL Database
* Add logging and audit tables
* Implement incremental file processing
* Move processed files to archive folder

---

## 📌 Conclusion

This project demonstrates how to build a **scalable and reusable data ingestion pipeline** using Azure Data Factory, which is a common pattern used in real-world data engineering workflows.

---
