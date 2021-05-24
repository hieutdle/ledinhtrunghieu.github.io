---
layout: post
author: ledinhtrunghieu
title: Lesson 15 - Introduction to Relational Databases in SQL
---

# 1. Introduction to relational databases

## 1.1. First Example
**A relational database:**
* Real-life entities become tables.
* Reduced redundancy 
* Data integrity by relationships

```sql
SELECT table_schema, table_name 
FROM information_schema.tables;
```
<img src="/assets/images/20210506_IntroRDInSQL/pic1.png" class="largepic"/>

```sql
SELECT table_name, column_name, data_type FROM information_schema.columns
WHERE table_name = 'pg_config';
```
<img src="/assets/images/20210506_IntroRDInSQL/pic2.png" class="largepic"/>

`information_schema` is a meta-database that holds information about your current database. `information_schema` has multiple tables you can query with the known **SELECT * FROM** syntax:
* `tables`: information about all tables in your current database
* `columns`: information about all columns in all of the tables in your current database


**Count Column**
```sql
SELECT COUNT(*)
  FROM information_schema.columns
 WHERE table_catalog = 'database_name' -- the database
   AND table_name = 'table_name'
```

How many columns does the table `university_professors` have?


```sql
SELECT COUNT(*)
  FROM information_schema.columns
 WHERE table_schema = 'public' -- the database
   AND table_name = 'university_professors'
```

## 1.2. Tables

**One "entity type" in the database**

<img src="/assets/images/20210506_IntroRDInSQL/pic3.png" class="largepic"/>

**Better database with four entity types**

<img src="/assets/images/20210506_IntroRDInSQL/pic4.png" class="largepic"/>

**Create new tables with CREATE TABLE**

```sql
CREATE TABLE weather ( 
    clouds text, 
    temperature numeric, 
    weather_station char(5)
);
```

**ADD a COLUMN with ALTER TABLE**

```sql

ALTER TABLE table_name
ADD COLUMN column_name data_type;

-- Add the university_shortname column
Alter table professors
Add column university_shortname text;

```

## 1.3. Update your database as the structure changes

**Current database model**

<img src="/assets/images/20210506_IntroRDInSQL/pic5.png" class="largepic"/>

**Only store DISTINCT data in new tables**
```sql
SELECT COUNT(*)
FROM university_professors;

1377

SELECT COUNT(DISTINCT organization) 
FROM university_professors;

1287
```

**INSERT DISTINCT records INTO the new tables**

```sql
--The INSERT INTO statement manually

INSERT INTO table_name (column_a, column_b) 
VALUES ("value_a", "value_b");

INSERT INTO organizations 
SELECT DISTINCT organization,
    organization_sector 
FROM university_professors;
```

**RENAME a COLUMN in affiliations**

```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

**A professor is uniquely identified by firstname,lastname only**

```sql
SELECT DISTINCT firstname, lastname, university_shortname
FROM university_professors 
ORDER BY lastname;

SELECT DISTINCT firstname, lastname
FROM university_professors
ORDER BY lastname;

same (551 records)
```
<img src="/assets/images/20210506_IntroRDInSQL/pic6.png" class="largepic"/>

So the `university_shortname` column is not needed in order to reference a professor in the affiliations table. You can remove it, and this will reduce the redundancy in your database again


**DROP a COLUMN in affiliations**
```sql
ALTER TABLE table_name 
DROP COLUMN column_name;
```

**Delete tables with DROP TABLE**
```sql
DROP TABLE table_name;
```
# 2. Enforce data consistency with attribute constraints

Specify data types in columns, enforce column uniqueness, and disallow NULL values in this chapter.

## 2.1. Better data quality with constraints

**Intergrity constraints**
1. Attribute constraints, e.g. data types on columns 
2. Key constraints, e.g. primary keys 
3. Referential integrity constraints, enforced through foreign keys 

**Why constraints?**
* Constraints give the data structure (With good constraints in plce, people who type in birthdates, for example, have to enter them in always the same form)
* Constraints help with consistency, and thus data quality (meaning that a row in a certain table has exactly the same form as the next row, and so forth)
* Data quality is a business advantage / data science prerequisite 
* Enforcing is difficult, but PostgreSQL helps

**PostgreSQL data types as attribute constraints**

<img src="/assets/images/20210506_IntroRDInSQL/pic7.png" class="largepic"/>

**Dealing with data types (casting)**
```sql
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill 
FROM weather;
```

## 2.2 Working with data types

**Working with data types**
* Enforced on columns (i.e. attributes) 
* Define the so-called "domain" of a column (what form these values can take – and what not)
* Define what operations are possible (a street number will always be an actual number, and a postal code will always have no more than 6 digits)
* Enfore consistent storage of values

**The most common types**
* `text`: character strings of any length
* `varchar[(x)]`: a maximum of `n`characters
* `char[(x)]`: a fixed-length string of `n` characters
* `boolean`: can only take three states, e.g. `TRUE`,`FALSE` and `NULL`
* `date`,`time` and `timestamp`: various formats for date and time calculations
* `numeric` : arbitrary precision numbers, e.g. `3.1457`
* `integer`: whole numbers in the range of `-2147483648` and `++2147483647`

**CHAR vs VARCHAR**
* CHAR
    1. Used to store character string value of fixed length.
    2 .The maximum no. of characters the data type can hold is 255 characters.
    3. It's 50% faster than VARCHAR.
    4. Uses static memory allocation.
* VARCHAR
    1. Used to store variable length alphanumeric data.
    2. The maximum this data type can hold is up to
    3. Pre-MySQL 5.0.3: 255 characters.
    4. Post-MySQL 5.0.3: 65,535 characters shared for the row.
    5. It's slower than CHAR.
    6. Uses dynamic memory allocation.

```sql
Create table temp
(City CHAR(10),
Street VARCHAR(10));

Insert into temp
values('Pune','Oxford');

select length(city), length(street) from temp;
Output will be

length(City)          Length(street)
10                    6
```
**Specifying types upon table creation**

```sql
CREATE TABLE students ( 
    ssn integer,
    name varchar(64), 
    dob date,
    average_grade numeric(3, 2), -- e.g. 5.54 
    tuition_paid boolean
);
```

**Alter types after table creation**
```sql
ALTER TABLE students 
ALTER COLUMN name
TYPE varchar(128);

ALTER TABLE students
ALTER COLUMN average_grade 
TYPE integer
-- Turns 5.54 into 6, not 5, before type conversion 
USING ROUND(average_grade);
```

```sql
-- Change the type of firstname
ALTER TABLE professors
ALTER COLUMN firstname
TYPE varchar(64);

-- Convert the values in firstname to a max. of 16 characters
ALTER TABLE professors 
ALTER COLUMN firstname 
TYPE varchar(16)
USING SUBSTRING(firstname  FROM 1 FOR 16)
```

## 2.3. The not-null and unique constraints

**The not-null constraint**
* Disallow `NULL` values in a certain column
* Must hold true for the current state 
* Must hold true for any future state

**What does NULL mean?**
* unknown
* does not exist
* does not apply

**Add or remove a not-null constraint**
```sql
CREATE TABLE students ( 
    ssn integer not null,
    lastname varchar(64) not null, 
    home_phone integer, 
    office_phone integer
);

ALTER TABLE students 
ALTER COLUMN home_phone 
SET NOT NULL;

ALTER TABLE students 
ALTER COLUMN ssn 
DROP NOT NULL;
```

**The Unique constraints**
* Disallow duplicate values in a column 
* Must hold true for the current state 
* Must hold true for any future state

**Adding unique constraints**
```sql
CREATE TABLE table_name ( 
    column_name UNIQUE
);

ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name);
```

# 3. Uniquely identify records with key constraints

Add primary and foreign keys to the tables. 

## 3.1. Keys and superkeys

**The database model with primary keys**

<img src="/assets/images/20210506_IntroRDInSQL/pic8.png" class="largepic"/>

**What is a key?**
* Attribute(s) that identify a record uniquely
* As long as attributes can be removed: superkey
* If no more attributes can be removed: minimal superkey or key

<img src="/assets/images/20210506_IntroRDInSQL/pic9.png" class="largepic"/>
* SK1 = {license_no, serial_no, make, model, year} 
* SK2 = {license_no, serial_no, make, model}
* SK3 = {make, model, year}, SK4 = {license_no, serial_no}, SKi, ..., SKn

The combination of all attributes is a superkey. If we remove the "year" attribute from the superkey, the six records are still unique, so it's still a superkey.

However, there are only four minimal superkeys:
K1 =  = {license_no}; K2 = {serial_no}; K3 = {model}; K4 = {make, year}
* K1 to 3 only consist of one attribute
* Removing either "make" or "year" from K4 would result in duplicates 
* Only one candidate key can be the chosen key

These four minimal superkeys are also called **candidate keys**. Why candidate keys? In the end, there can only be one key for the table, which has to be chosen from the candidates

**Practice**
1. Count the distinct records for all possible combinations of columns. If the resulting number x equals the number of all rows in the table for a combination, you have discovered a superkey.
2. Then remove one column after another until you can no longer remove columns without seeing the number x decrease. If that is the case, you have discovered a (candidate) key.

```sql
-- Try out different combinations
SELECT COUNT(DISTINCT(firstname,lastname)) 
FROM professors;
```

## 3.2. Primary keys

**Primary keys**
* One primary key per database table, chosen from candidate keys \
* Uniquely identifies records, e.g. for referencing in other tables Unique and not-null constraints both apply
Primary keys are time-invariant: choose columns wisely!
