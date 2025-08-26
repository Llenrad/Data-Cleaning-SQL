# HR Data Cleaning with MySQL

This repository contains SQL scripts used to clean and transform an HR dataset within a MySQL environment. The process includes column renaming, date formatting, data type updates, and age calculation—all essential steps for preparing the data for analysis or dashboard integration.

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [SQL Cleaning Steps](#sql-cleaning-steps)
3. [Key Transformations](#key-transformations)
4. [Tools Used](#tools-used)
5. [Disclaimer](#disclaimer)

---

## Project Overview

The SQL script operates on a database named `project`, specifically targeting the `hr` table. The goal is to standardize date formats, rename columns, and enrich the dataset with calculated fields such as employee age.

---

## SQL Cleaning Steps

```sql
USE project;

-- Disable Safe Update Mode
SET sql_safe_updates = 0;

-- 📋 Preview Table
SELECT * FROM hr;
DESCRIBE hr;

-- Rename Column
ALTER TABLE hr
CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;

-- Format Birthdate
UPDATE hr
SET birthdate = CASE
  WHEN birthdate LIKE '%/%' THEN date_format(str_to_date(birthdate,'%m/%d/%Y'),'%Y-%m-%d') 
  WHEN birthdate LIKE '%-%' THEN date_format(str_to_date(birthdate,'%m-%d-%Y'),'%Y-%m-%d')
  ELSE NULL
END;
ALTER TABLE hr
MODIFY COLUMN birthdate DATE;

-- Format Hire Date
UPDATE hr
SET hire_date = CASE
  WHEN hire_date LIKE '%/%' THEN date_format(str_to_date(hire_date,'%m/%d/%Y'),'%Y-%m-%d') 
  WHEN hire_date LIKE '%-%' THEN date_format(str_to_date(hire_date,'%m-%d-%Y'),'%Y-%m-%d')
  ELSE NULL
END;
ALTER TABLE hr
MODIFY COLUMN hire_date DATE;

-- Format Termination Date
UPDATE hr
SET termdate = date(str_to_date(termdate, '%Y-%m-%d %H:%i:%s UTC'))
WHERE termdate IS NOT NULL AND termdate <>'';
ALTER TABLE hr
MODIFY COLUMN termdate DATE;

-- Add Age Column
ALTER TABLE hr ADD COLUMN age INT;

-- Calculate Age
UPDATE hr
SET age = TIMESTAMPDIFF(YEAR, birthdate, '2023-04-06');

-- Verify Output
SELECT birthdate, age FROM hr;

```
---

## Key Transformations

- Renamed column `ï»¿id` to `emp_id`
- Standardized `birthdate`, `hire_date`, and `termdate` formats to `YYYY-MM-DD`
- Converted date columns to proper `DATE` data type
- Added and calculated `age` column using `TIMESTAMPDIFF`

---

## Tools Used

- **MySQL Server** – SQL scripting and transformation  
- **SQL Workbench / CLI** – Execution environment

---

## Disclaimer

This project is for portfolio demonstration purposes only. The dataset used is fictional or publicly available and does not represent any real company or personal records.

