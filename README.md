# SQL-Data-cleaning

This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

## 1. Data Introduction
```SQL
SELECT 
	* 
FROM 
	club_member_info cmi 
LIMIT 
	10;
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

## 2. Copy Table
### 2.1 Create a new table for cleaning
```SQL
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

### 2.2 Copy all values from original table
```SQL
-- import_data_infor
INSERT INTO 
	club_member_info_cleaned 
SELECT 
	* 
FROM 
	club_member_info ; 
```

## 3 Overview number of existing rows before making "UPDATE" statement
```SQL
SELECT 
	count(*) AS number_of_rows
FROM 
	club_member_info;
```
|number_of_rows|
|--------------|
|2010|

### 3.1 Structure and clean "full_name" column
```SQL
-- trim_full_name_column
UPDATE 
	club_member_info_cleaned 
SET 
	full_name = TRIM(full_name);

-- capitalize_full_name_column
UPDATE 
	club_member_info_cleaned 
SET 
	full_name = UPPER(full_name);
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|

### 3.2 Check and clean "age" column
```SQL
SELECT 
	age AS odd_age
FROM 
	club_member_info_cleaned cmic 
WHERE 
	age >=90;
```
|odd_age|
|---|
|399|
|555|
|544|
|499|
|522|
||
|277|
||
|288|
|588|
|599|
|677|
|322|
|644|
||
|411|
|633|
|222|

```SQL
-- clean_age_column
UPDATE 
	club_member_info_cleaned 
SET 
	age = SUBSTR(age, 1, 2);
```

```SQL
SELECT 
	age,
	count(*) AS age_volume
FROM 
	club_member_info_cleaned cmic 
GROUP BY
	age
ORDER BY 
	age_volume 
DESC
LIMIT 
	1;
```
|age|age_volume|
|---|----------|
|40|66|

```SQL
-- fill_in_blank
UPDATE 
	club_member_info_cleaned 
SET 
	age = 40
WHERE 
	age = '';
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|

### 3.3 Check and clean "martial_status" column
```SQL
SELECT 
	DISTINCT martial_status 
FROM 
	club_member_info_cleaned cmic;
```
|martial_status|
|--------------|
|married|
|divorced|
||
|single|
|divored|

```SQL
-- fill_in_blank
UPDATE 
	club_member_info_cleaned 
SET 
	martial_status = 'single' 
WHERE 
	martial_status = '';

-- clean_martial_status_column
UPDATE 
	club_member_info_cleaned 
SET 
	martial_status = 'divorced' 
WHERE 
	martial_status = 'divored';
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35|single|co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|

### 3.4 Check and clean "membership_date" column
```SQL
SELECT 
	DISTINCT CAST(SUBSTRING(membership_date, -4, 4) AS INT) AS wrong_membership_date
FROM 
	club_member_info_cleaned cmic
WHERE 
	wrong_membership_date > 2000 
ORDER BY 
	wrong_membership_date;
```
|wrong_membership_date|
|---------------------|
|1912|
|1913|
|1914|
|1915|
|1916|
|1917|
|1919|
|1921|

#### Test Syntax before making "UPDATE"
```SQL
SELECT 
	SUBSTR(membership_date, 1, LENGTH(membership_date) - 4) || '20' || SUBSTR(membership_date, LENGTH(membership_date) -1,2) AS tested_membership_date
FROM 
	club_member_info_cleaned cmic
WHERE 
	membership_date LIKE '%19__';
```
|tested_membership_date|
|----------------------|
|3/12/2021|
|10/1/2012|
|2/20/2016|
|5/8/2012|
|10/4/2019|
|3/10/2013|
|1/8/2012|
|9/2/2014|
|5/11/2016|
|1/31/2015|
|5/15/2015|
|3/3/2017|
|4/30/2015|
|5/22/2021|
|10/27/2015|
|7/5/2015|

```SQL
-- clean_membership_date_column
UPDATE 
	club_member_info_cleaned 
SET 
	membership_date = SUBSTR(membership_date, 1, LENGTH(membership_date) - 4) || '20' || SUBSTR(membership_date, LENGTH(membership_date) -1,2)
WHERE 
	membership_date LIKE '%19__';
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35|single|co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|

### 3.5 Check "membership_date" column
```SQL
SELECT
	email,
	count(*) AS duplicates
FROM
	club_member_info_cleaned cmic
GROUP BY
	email
HAVING
	count(*) > 1;
```
|email|duplicates|
|-----|----------|
|ehuxterm0@marketwatch.com|3|
|gprewettfl@mac.com|2|
|greglar4r@answers.com|2|
|hbradenri@freewebs.com|2|
|mmorralleemj@wordpress.com|2|
|nfilliskirkd5@newsvine.com|2|
|omaccaughen1o@naver.com|2|
|slamble81@amazon.co.uk|2|
|tdunkersley8u@dedecms.com|2|

#### Using the row ID to delete unwanted duplicate
```SQL
DELETE FROM club_member_info_cleaned
WHERE rowid NOT IN (
    	SELECT 
		MIN(rowid) as min_id
	FROM 
		club_member_info_cleaned cmic
	GROUP BY email
	);
```
The results:

```SQL
SELECT 
	count(*) AS number_of_rows_updated
FROM 
	club_member_info_cleaned;
```
|number_of_rows_updated|
|--------------|
|2000|

```SQL
SELECT 
	email,
	COUNT(*) AS duplicates
FROM 
	club_member_info_cleaned cmic
GROUP BY 
	email
HAVING 
	COUNT(*) > 1;
```
|email|duplicates|
|-----|----------|

