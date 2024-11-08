# Data Cleaning Project for Fraud Detection
## Description:
This repository contains a data cleaning project using MySQL to prepare a transaction dataset for fraud detection and identifying invalid payments. The dataset has been processed to remove inconsistencies, handle missing values, and structure the data for further analysis in fraud detection models.
## Table of Contents
# Table of Contents

1. [Project Overview](#project-overview)
2. [Technologies Used](#technologies-used)
3. [Dataset](#dataset)
4. [Data Cleaning Process](#data-cleaning-process)
5. [How to Run the Project](#how-to-run-the-project)
6. [License](#license)

## Project Overview:
The goal of this project is to clean and preprocess a transaction dataset to enable fraud detection and identify invalid payments. The dataset includes transactional data with various potential issues like missing values, duplicates, and inconsistent data types, which are resolved through MySQL-based cleaning techniques.

## Technologies used:
* **MySQL** for data cleaning and preprocessing.
* **SQL Scripts** to handle data inconsistencies, missing values, and duplicates.
* **Google Colab** for running the code and MySQL queries.

## Dataset

The dataset used in this project is a transaction dataset that includes transaction details such as transaction ID, amount and other features. The data has been preprocessed to ensure its readiness for fraud detection algorithms.

### Dataset Columns:

- **step**: Time step of the transaction.
- **type**: Type of transaction (e.g., CASH-IN, CASH-OUT, PAYMENT).
- **amount**: Amount of the transaction.
- **nameOrig**: Account ID of the origin (payer).
- **oldbalanceOrg**: The balance of the origin account before the transaction.
- **newbalanceOrig**: The balance of the origin account after the transaction.
- **nameDest**: Account ID of the destination (payee).
- **oldbalanceDest**: The balance of the destination account before the transaction.
- **newbalanceDest**: The balance of the destination account after the transaction.
- **isFraud**: A label indicating if the transaction is fraudulent (1) or not (0).
- **isFlaggedFraud**: A flag indicating if the transaction was flagged as suspicious by the system (1 if flagged, 0 if not).

- ### Data Cleaning Process

The data cleaning steps include:

1. **Handling Missing Values**: Identifying and handling null or missing values in critical fields.
2. **Removing Duplicates**: Ensuring each transaction is unique and eliminating duplicate rows.
3. **Converting Data Types**: Ensuring that each column is in the correct format (e.g., converting text to date format, numeric columns to integers).
4. **Identifying and Handling Outliers**: Filtering out anomalous transactions that could affect the fraud detection model.
5. **Validating Payment Data**: Identifying invalid payments or errors in the transaction data.

## How to Run the Project

Follow these steps to run the project on your local machine:

### 1. **Clone the Repository**

Initial Setup and Cloning

Clone this repository:

```bash
git clone https://github.com/your-username/mysql-data-cleaning.git
```
Set up MySQL if it’s not already installed. Import your dataset to start working on data cleaning.

-----

## 2. Initial Exploration

View All Data:
```sql
SELECT * FROM locard_comms.transactions_1;
```

Create a Backup Table for Safety:

```sql
CREATE TABLE transactions_2 AS
SELECT * FROM transactions_1;
```

Check for Missing Values:

```sql
SELECT * FROM transactions_1
WHERE step IS NULL OR type IS NULL OR amount IS NULL 
OR nameOrig IS NULL OR oldbalanceOrg IS NULL 
OR newbalanceOrig IS NULL OR nameDest IS NULL 
OR oldbalanceDest IS NULL OR newbalanceDest IS NULL 
OR isFraud IS NULL OR isFlaggedFraud IS NULL;
```

Check Column Information and Data Types:

```sql
DESCRIBE transactions_1;
```

Set Boolean Columns Using `TINYINT(1):`
```sql
ALTER TABLE transactions_1 MODIFY COLUMN isFraud TINYINT(1) NOT NULL DEFAULT 0;
ALTER TABLE transactions_1 MODIFY COLUMN isFlaggedFraud TINYINT(1) NOT NULL DEFAULT 0;
```
---

## 3. Basic Data Cleaning and Summary Analysis

Calculate Summary Statistics for Numeric Columns:
```sql
SELECT AVG(amount), MIN(amount), MAX(amount), 
AVG(oldbalanceOrg), AVG(newbalanceOrig), 
AVG(oldbalanceDest), AVG(newbalanceDest) 
FROM transactions_1;
```

Identify Suspicious Transactions:
```sql
SELECT * FROM locard_comms.transactions_1 WHERE amount = 0.32;
```

Check for Duplicate Records:
```sql
SELECT nameOrig, nameDest, COUNT(*)
FROM transactions_1
GROUP BY step, amount, nameOrig, nameDest
HAVING COUNT(*) > 1;
```

## 4. Structuring Data for Deeper Analysis

Set Primary Keys:
```sql
ALTER TABLE transactions_1 ADD PRIMARY KEY (nameOrig(50), nameDest(50));
```

Create a Subset Table for Payment Data:
```sql
CREATE TABLE payment AS
SELECT * FROM transactions_1 WHERE type = 'payment';
```

## 5. Further Cleaning and exploratin on Payment Table
Check for Null Values in `payment` Table:
```sql
SELECT * FROM payment WHERE step IS NULL OR type IS NULL 
OR amount IS NULL OR nameOrig IS NULL 
OR oldbalanceOrg IS NULL OR newbalanceOrig IS NULL 
OR nameDest IS NULL OR oldbalanceDest IS NULL 
OR newbalanceDest IS NULL OR isFraud IS NULL 
OR isFlaggedFraud IS NULL;
```

Verify Clean Payment Records:
```sql
SELECT * FROM payment WHERE newbalanceOrig = oldbalanceOrg - amount;
```

Identify Fraudulent Transactions:
```sql
SELECT * FROM payment WHERE newbalanceOrig = oldbalanceOrg - amount AND isFraud = 1;
```

Create a `clean_payment` Table for Clean Records:
```sql
CREATE TABLE clean_payment AS
SELECT * FROM payment WHERE newbalanceOrig = oldbalanceOrg - amount;
```

Move Similar Records to `clean_payment` with Minor Discrepancies:
```sql
INSERT INTO clean_payment
SELECT * FROM payment
WHERE ABS(newbalanceOrig - (oldbalanceOrg - amount)) <= 1.00;
```

Delete Records from payment That Were Added to clean_payment:
```sql
DELETE FROM payment WHERE ABS(newbalanceOrig - (oldbalanceOrg - amount)) <= 1.00;
```

## 6. Identify and Separate Invalid Payments
Identify Invalid Payments:
```sql
SELECT * FROM payment WHERE amount > oldbalanceOrg;
```

Create `invalid_payment` Table from Invalid Records:
```sql
CREATE TABLE invalid_payment AS SELECT * FROM payment WHERE amount > oldbalanceOrg;
```

Remove Invalid Payments from payment Table:
```sql
DELETE FROM payment WHERE amount > oldbalanceOrg;
```

Create and Populate Fraud Table:
```sql
CREATE TABLE fraud AS SELECT * FROM payment;
```

### License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
