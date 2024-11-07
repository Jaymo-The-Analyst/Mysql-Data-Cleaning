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

### How to Run the Project

Follow these steps to run the project on your local machine:

### 1. **Clone the Repository**

Clone the repository to your local machine using Git:

``bash
git clone https://github.com/Jaymo-The-Analyst/Mysql-Data-Cleaning.git


### 2. **Set Up MySQL Database**
Ensure that you have MySQL installed on your local machine or set up a MySQL instance in the cloud.

- If MySQL is not installed, you can download it from [here](https://dev.mysql.com/downloads/installer/).
- Create a new database for the project (e.g., `fraud_detection`):

```sql
CREATE DATABASE fraud_detection;









### 2. **Set Up MySQL Database**

Ensure that you have MySQL installed on your local machine or set up a MySQL instance in the cloud.

- If MySQL is not installed, you can download it from [here](https://dev.mysql.com/downloads/installer/).
- Create a new database for the project (e.g., `fraud_detection`):

```sql
CREATE DATABASE fraud_detection;











### License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
