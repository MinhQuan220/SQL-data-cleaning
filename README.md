# SQL-data-cleaning

This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

## Data Introduction
```SQL
SELECT * FROM club_member_info cmi LIMIT 10;
```
The result:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## Some issues with the data
'full_name' column
- Inconsistent letter case
- Leading and trailing whitespaces

'age' column
- Age out of realistic range

'martial_status' column
- Spelling errors
- Data has blank cells

'membership_date' column
- Year out of realistic range
## Step 1: Copy table
### Create new table for cleaning
```SQL
-- club_member_info definition
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
### Copy all values from original table
```SQL
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;
```
## Step 2: Customize and standardize the 'full_name' column data
### Remove leading and trailing spaces in column 'full_name'
```SQL
UPDATE club_member_info_cleaned  SET full_name = TRIM(full_name);
```
### Normalize data: Convert column 'full_name' to all uppercase.
```SQL
UPDATE club_member_info_cleaned  SET full_name = UPPER(full_name);
```
The result:
|full_name|
|---------|
|ADDIE LUSH|
|ROCK CRADICK|
|SYDEL SHARVELL|
|CONSTANTIN DE LA CRUZ|
|GAYLOR REDHOLE|
|WANDA DEL MAR|
|JOANN KENEALY|
|JOETE CUDIFF|
|MENDIE ALEXANDRESCU|
|FEY KLOSS|

## Step 3: Customize and standardize the 'age' column data
Hypothesis: when entering data, it was entered incorrectly because the last number was double-clicked when entering
### Correct Unrealistic Age Values ​​in the Column
```SQL
UPDATE club_member_info_cleaned
SET age = SUBSTR(age, 1,2)
WHERE LENGTH(age) = 3;
```
The result:
```SQL
SELECT DISTINCT age  FROM club_member_info_cleaned;
```
|age|
|---|
|18|
|19|
|20|
|21|
|22|
|23|
|24|
|25|
|26|
|27|
|28|
|29|
|30|
|31|
|32|
|33|
|34|
|35|
|36|
|37|
|38|
|39|
|40|
|41|
|42|
|43|
|44|
|45|
|46|
|47|
|48|
|49|
|50|
|51|
|52|
|53|
|54|
|55|
|56|
|57|
|58|
|59|
|60|
|61|
|62|
|63|
|64|
|65|
|66|
|67|
|68|
||

