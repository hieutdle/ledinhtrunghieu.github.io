---
layout: post
author: ledinhtrunghieu
title: HackerRank SQL
---


# 1. Basic Select

# 1.1 Weather Observation Station Problems

The STATION table is described as follows:
<img src="/assets/images/20211006_HackerRankSQL/pic1.png" class="largepic"/>
where LAT_N is the northern latitude and LONG_W is the western longitude.

**Weather Observation Station 3**

Query a list of CITY names from STATION for cities that have an even ID number. Print the results in any order, but exclude duplicates from the answer.

```sql
SELECT DISTINCT(CITY)
FROM STATION
WHERE ID%2=0 or MOD(ID,2) = 0
ORDER BY CITY;
```

**Weather Observation Station 5**
Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

```sql
SELECT CITY, LENGTH(CITY) AS L
FROM STATION
ORDER BY L ASC, CITY
LIMIT 1;
SELECT CITY, LENGTH(CITY) as L
FROM STATION
ORDER BY L DESC, CITY
LIMIT 1;
```

**Weather Observation Station 6**
Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.

```sql
SELECT DISTINCT(CITY)
FROM STATION
WHERE LEFT(CITY,1) IN ('A','E','O','U','I');
```

**Weather Observation Station 9**
Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.
```sql
SELECT DISTINCT(CITY)
FROM STATION
WHERE LEFT(CITY,1) NOT IN ('A','E','O','U','I');
```
Consider  and  to be two points on a 2D plane.
happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points  and  and round it to a scale of  decimal places.

**Weather Observation Station 15**
Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to  decimal places.
```sql
SELECT ROUND(LONG_W,4)
FROM STATION
WHERE LAT_N = (SELECT MAX(LAT_N)
                FROM STATION
                WHERE LAT_N < 137.2345)
```
or
```sql
SELECT ROUND(LONG_W, 4) 
FROM STATION 
WHERE LAT_N < 137.2345 
ORDER BY LAT_N 
DESC LIMIT 1;
```
**Weather Observation Station 18**
<img src="/assets/images/20211006_HackerRankSQL/pic3.png" class="largepic"/>
```sql
SELECT
    ROUND(ABS(MAX(LAT_N)  - MIN(LAT_N))
        + ABS(MAX(LONG_W) - MIN(LONG_W)), 4)
FROM 
    STATION;
```

**Weather Observation Station 19**
<img src="/assets/images/20211006_HackerRankSQL/pic11.png" class="largepic"/>
```sql
SELECT
    ROUND(SQRT(POWER(ABS(MAX(LAT_N)  - MIN(LAT_N)),2)+ POWER(ABS(MAX(LONG_W)-MIN(LONG_W)),2)),4)
FROM 
    STATION;
```


**Higher Than 75 Marks**
<img src="/assets/images/20211006_HackerRankSQL/pic2.png" class="largepic"/>

Query the Name of any student in STUDENTS who scored higher than  Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.
```sql
SELECT Name
FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name,3),ID;
```

**Employee Salaries**

Oceania 109190
South America 147435
Europe 175138
Africa 274439
Asia 693038

Oceania 109189 
South America 147435 
Europe 175138 
Africa 274439 
Asia 693038 
# 2. Aggeration

**Weather Observation Station 20**
A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.

```sql
SET @N:=0;
SELECT COUNT(*) FROM STATION INTO @TOTAL;
SELECT
    ROUND(AVG(A.LAT_N), 4)
FROM (SELECT @N := @N +1 AS ROW_ID, LAT_N FROM STATION ORDER BY LAT_N) A
WHERE
    CASE WHEN MOD(@TOTAL, 2) = 0 
            THEN A.ROW_ID IN (@TOTAL/2, (@TOTAL/2+1))
            ELSE A.ROW_ID = (@TOTAL+1)/2
    END
;
```


**The Blunder**
Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's  key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary. Write a query calculating the amount of error , and round it up to the next integer.
<img src="/assets/images/20211006_HackerRankSQL/pic6.png" class="largepic"/>
<img src="/assets/images/20211006_HackerRankSQL/pic7.png" class="largepic"/>
<img src="/assets/images/20211006_HackerRankSQL/pic8.png" class="largepic"/>

```sql
SELECT CEIL(AVG(Salary)-AVG(REPLACE(Salary,'0','')))
FROM EMPLOYEES
```


**Top Earners**
<img src="/assets/images/20211006_HackerRankSQL/pic10.png" class="largepic"/>
<img src="/assets/images/20211006_HackerRankSQL/pic9.png" class="largepic"/>

```sql
SELECT MAX(SALARY*MONTHS), COUNT(*)
FROM EMPLOYEE
WHERE (SALARY*MONTHS) = (SELECT MAX(SALARY*MONTHS)
                         FROM EMPLOYEE);
```


# 3. Advance Select


**The PADS**
<img src="/assets/images/20211006_HackerRankSQL/pic5.png" class="largepic"/>

Generate the following two result sets:

Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:

```
There are a total of [occupation_count] [occupation]s.
```

where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

Note: There will be at least two entries in the table for each type of occupation.
Sample output
```sql
Ashely(P)
Christeen(P)
Jane(A)
Jenny(D)
Julia(A)
Ketty(P)
Maria(A)
Meera(S)
Priya(S)
Samantha(D)
There are a total of 2 doctors.
There are a total of 2 singers.
There are a total of 3 actors.
There are a total of 3 professors.
```
```sql
SELECT CONCAT(NAME,'(',SUBSTR(OCCUPATION,1,1),')') AS N
FROM OCCUPATIONS
ORDER BY N;

SELECT CONCAT('There are a total of ',COUNT(OCCUPATION),' ',LOWER(OCCUPATION),'s.')
FROM OCCUPATIONS
GROUP BY OCCUPATION
ORDER BY COUNT(OCCUPATION), OCCUPATION;
```

**Occupations**
Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.

Note: Print NULL when there are no more names corresponding to an occupation.
Input:
<img src="/assets/images/20211006_HackerRankSQL/pic15.png" class="largepic"/>
Output:
```sql
Jenny    Ashley     Meera  Jane
Samantha Christeen  Priya  Julia
NULL     Ketty      NULL   Maria
```

The first column is an alphabetically ordered list of Doctor names.
The second column is an alphabetically ordered list of Professor names.
The third column is an alphabetically ordered list of Singer names.
The fourth column is an alphabetically ordered list of Actor names.

```sql
SET @D:=0,@P:=0,@S:=0,@A:=0;
SELECT MIN(Doctor),MIN(Professor),MIN(Singer),MIN(Actor)FROM 
(SELECT CASE 
            When Occupation = "Doctor" then @D := @D+1
            When Occupation = "Professor" then @P := @P+1
            When Occupation = "Singer" then @S := @S+1
            When Occupation = "Actor" then @A := @A+1
        END AS RowNumber,
        CASE When Occupation = "Doctor" then Name end as Doctor,
        CASE When Occupation = "Professor" then Name end as Professor,
        CASE When Occupation = "Singer" then Name end as Singer,
        CASE When Occupation = "Actor" then Name end as Actor
    FROM OCCUPATIONS
    ORDER BY Name) as Temp
GROUP BY RowNumber;
```



**Type of Triangle**

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:

Equilateral: It's a triangle with 3 sides of equal length.
Isosceles: It's a triangle with 2 sides of equal length.
Scalene: It's a triangle with 3 sides of differing lengths.
Not A Triangle: The given values of A, B, and C don't form a triangle.

<img src="/assets/images/20211006_HackerRankSQL/pic4.png" class="largepic"/>

```sql
SELECT
  CASE 
    WHEN A + B <= C or A + C <= B or B + C <= A THEN 'Not A Triangle'
    WHEN A = B and B = C THEN 'Equilateral'
    WHEN A = B or A = C or B = C THEN 'Isosceles'
    WHEN A <> B and B <> C THEN 'Scalene'
  END tuple
FROM TRIANGLES;
```

**New Companies**

<img src="/assets/images/20211006_HackerRankSQL/pic16.png" class="largepic"/>
Input:
<img src="/assets/images/20211006_HackerRankSQL/pic17.png" class="largepic"/>
Output:
```sql
C1 Monika 1 2 1 2
C2 Samantha 1 1 2 2
```
Note:
The tables may contain duplicate records.
The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

```sql
SELECT  c.company_code, c.founder, 
        COUNT(DISTINCT l.lead_manager_code), COUNT(DISTINCT s.senior_manager_code),
        COUNT(DISTINCT m.manager_code), COUNT(DISTINCT e.employee_code)
FROM Company c, Lead_Manager l, Senior_Manager s, Manager m, Employee e
WHERE c.company_code = l.company_code AND 
      l.lead_manager_code = s.lead_manager_code AND
      s.senior_manager_code = m.senior_manager_code AND
      m.manager_code = e.manager_code
GROUP BY c.company_code,c.founder ORDER BY c.company_code;
```
Solution 2:
```sql
SELECT c.company_code, c.founder, 
       COUNT(DISTINCT l.lead_manager_code), COUNT(DISTINCT s.senior_manager_code),
       COUNT(DISTINCT m.manager_code), COUNT(DISTINCT e.employee_code)
FROM Company c JOIN Lead_Manager l ON c.company_code = l.company_code JOIN
     Senior_Manager s ON l.lead_manager_code = s.lead_manager_code JOIN
     Manager m ON s.senior_manager_code = m.senior_manager_code JOIN
     Employee e ON m.manager_code = e.manager_code   
GROUP BY c.company_code, c.founder ORDER BY c.company_code;
```

# 4. Basic Join

**The Report**

You are given two tables: Students and Grades.

<img src="/assets/images/20211006_HackerRankSQL/pic12.png" class="largepic"/>

<img src="/assets/images/20211006_HackerRankSQL/pic13.png" class="largepic"/>

Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

<img src="/assets/images/20211006_HackerRankSQL/pic14.png" class="largepic"/>

Output:
```sql
Maria 10 99
Jane 9 81
Julia 9 88 
Scarlet 8 78
NULL 7 63
NULL 7 68
```

```sql
SELECT IF(g.Grade>=8,s.Name,NULL), g.Grade, s.Marks
FROM STUDENTS as s
JOIN GRADES as g
WHERE s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
ORDER BY g.Grade DESC, s.Name ASC, s.Marks ASC;
```

