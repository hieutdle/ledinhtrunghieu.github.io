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

**Binary Tree Nodes**
Sample Input:
<img src="/assets/images/20211006_HackerRankSQL/pic18.png" class="largepic"/>
Sample Output:
```sql
1 Leaf
2 Inner
3 Leaf
5 Root
6 Leaf
8 Inner
9 Leaf
```
You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.
Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:
Root: If node is root node.
Leaf: If node is leaf node.
Inner: If node is neither root nor leaf node.
Explain:
<img src="/assets/images/20211006_HackerRankSQL/pic19.png" class="largepic"/>
```sql
SELECT N,
        (Case
            When P IS NULL then 'Root'
            When N in (SELECT DISTINCT(P) FROM BST) then 'Inner'
            ELSE 'Leaf'
            END) As NodeType
FROM BST
ORDER BY N ASC;
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

**Ollivander's Inventory**
Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.

<img src="/assets/images/20211006_HackerRankSQL/pic20.png" class="largepic"/>

Sample output

```sql
9 45 1647 10
12 17 9897 10
1 20 3688 8
15 40 6018 7
19 20 7651 6
11 40 7587 5
10 20 504 5
18 40 3312 3
20 17 5689 3
5 45 6020 2
14 40 5408 1
```
Explanation
<img src="/assets/images/20211006_HackerRankSQL/pic21.png" class="largepic"/>
```sql
SELECT W.ID, P.AGE, W.COINS_NEEDED, W.POWER 
FROM WANDS AS W
JOIN WANDS_PROPERTY AS P
ON (W.CODE = P.CODE) 
WHERE P.IS_EVIL = 0 AND W.COINS_NEEDED = (SELECT MIN(COINS_NEEDED) 
                                          FROM WANDS AS X
                                          JOIN WANDS_PROPERTY AS Y 
                                          ON (X.CODE = Y.CODE) 
                                          WHERE X.POWER = W.POWER AND Y.AGE = P.AGE) 
ORDER BY W.POWER DESC, P.AGE DESC;
```

**Top Competitors**
Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.
<img src="/assets/images/20211006_HackerRankSQL/pic22.png" class="largepic"/>

Input:
<img src="/assets/images/20211006_HackerRankSQL/pic23.png" class="largepic"/>
Output

```
90411 Joe
```

```sql
SELECT s.hacker_id, h.name
FROM submissions s
JOIN hackers h ON s.hacker_id = h.hacker_id
JOIN challenges c ON s.challenge_id = c.challenge_id
JOIN difficulty d ON c.difficulty_level = d.difficulty_level
WHERE s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(s.hacker_id) > 1
ORDER BY COUNT(s.hacker_id) DESC, s.hacker_id ASC;
```

**Challenges**
Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

<img src="/assets/images/20211006_HackerRankSQL/pic24.png" class="largepic"/>

Output:
```
21283 Angela 6
88255 Patrick 5
96196 Lisa 1
```
<img src="/assets/images/20211006_HackerRankSQL/pic25.png" class="largepic"/>
```
12299 Rose 6
34856 Angela 6
79345 Frank 4
80491 Patrick 3
81041 Lisa 1
```
Explain:
<img src="/assets/images/20211006_HackerRankSQL/pic26.png" class="largepic"/>

```sql
SELECT h.hacker_id, h.name, COUNT(c.challenge_id) as challenges_created
FROM hackers h
JOIN challenges c ON h.hacker_id = c.hacker_id
GROUP BY h.hacker_id,h.name
HAVING challenges_created = 
    (SELECT COUNT(c1.challenge_id) 
     FROM challenges AS c1 
     GROUP BY c1.hacker_id 
     ORDER BY COUNT(*) DESC LIMIT 1)
     OR challenges_created IN 
     (select t.cc
         from (select count(*) as cc
               from challenges
               group by hacker_id) t
         group by t.cc
         having count(t.cc) = 1)
ORDER BY challenges_created DESC,h.hacker_id;
```

**Contest Leaderboard**
You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!

The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of 0 from your result.
<img src="/assets/images/20211006_HackerRankSQL/pic27.png" class="largepic"/>

<img src="/assets/images/20211006_HackerRankSQL/pic28.png" class="largepic"/>

Output:
```sql
4071 Rose 191
74842 Lisa 174
84072 Bonnie 100
4806 Angela 89
26071 Frank 85
80305 Kimberly 67
49438 Patrick 43
```

Explain:
<img src="/assets/images/20211006_HackerRankSQL/pic29.png" class="largepic"/>

```sql
SELECT h.hacker_id, h.name, t1.total_score
  FROM (
        SELECT hacker_id, SUM(max_score) AS total_score
          FROM (
                SELECT hacker_id, MAX(score) AS max_score
                  FROM Submissions
                GROUP BY hacker_id, challenge_id
               ) t
        GROUP BY hacker_id
       ) t1
  JOIN Hackers h
    ON h.hacker_id = t1.hacker_id
 WHERE t1.total_score <> 0
 ORDER BY total_score DESC, hacker_id;
```




# 5. Advanced Join

**Symmetric Pairs**
<img src="/assets/images/20211006_HackerRankSQL/pic30.png" class="largepic"/>

```sql
SELECT f1.X, f1.Y FROM Functions f1
INNER JOIN Functions f2 ON f1.X=f2.Y AND f1.Y=f2.X
GROUP BY f1.X, f1.Y
HAVING COUNT(f1.X)>1 or f1.X<f1.Y
ORDER BY f1.X 
```
The criteria in the having clause allows us to prevent duplication in our output while still achieving our goal of finding mirrored pairs. We have to treat our pairs where f1.x = f1.y and f1.x <> f1.y differently to capture both. The first criteria handles pairs where f1.x = f1.y and the 2nd criteria handles pairs where f1.x <> f1.y, which is why the or operator is used.

The first part captures records where f1.x = f1.y. The 'count(f1.x) > 1' requires there to be at least two records of a mirrored pair to be pulled through. Without this a pair would simply match with itself (since it's already it's own mirrored pair) and be pulled through incorrectly when you join the table on itself.

The 2nd part matches the remaining mirrored pairs. It's important to note that for this challenge, the mirrored match of (f1.x,f1.y) is considered a duplicate and excluded from the final output. You can see this in the sample output where (20, 21) is outputted, but not (21,20). The 'or f1.x < f1.y' criteria allows us to pull all those pairs where f1.x does not equal f1.y, but where f1.x is also less than f1.y so we don't end up with the mirrored paired duplicate.

**SQL Project Planning**
<img src="/assets/images/20211006_HackerRankSQL/pic31.png" class="largepic"/>

Sample Output

```
2015-10-28 2015-10-29
2015-10-30 2015-10-31
2015-10-13 2015-10-15
2015-10-01 2015-10-04
```

Explanation

The example describes following four projects:

Project 1: Tasks 1, 2 and 3 are completed on consecutive days, so these are part of the project. Thus start date of project is 2015-10-01 and end date is 2015-10-04, so it took 3 days to complete the project.
Project 2: Tasks 4 and 5 are completed on consecutive days, so these are part of the project. Thus, the start date of project is 2015-10-13 and end date is 2015-10-15, so it took 2 days to complete the project.
Project 3: Only task 6 is part of the project. Thus, the start date of project is 2015-10-28 and end date is 2015-10-29, so it took 1 day to complete the project.
Project 4: Only task 7 is part of the project. Thus, the start date of project is 2015-10-30 and end date is 2015-10-31, so it took 1 day to complete the project.

First, we need to find start dates and end dates of each project separately. If a start date is not included in all end dates, it should be the start date of a project:
Similarly, an end date excluded in start date column should be the end date of some project:
Then, we need to find (start date, end date) pairs for each project. Before that, we can cross join start dates and end dates of projects to generate all potential pairs. Also, for the same project, end date should be the minimum among all end dates of projects that are larger than start date of the project:
Finally, sort output by duration and start date:

```sql
SELECT Start_Date, MIN(End_Date) FROM
(SELECT Start_Date FROM Projects WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)) AS s,
(SELECT End_Date FROM Projects WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)) AS e
WHERE Start_Date < End_Date
GROUP BY Start_Date
ORDER BY DATEDIFF(MIN(End_Date), Start_Date), Start_Date;
```



# 6. Alternative Queries

**Print Prime Numbers**
```sql
SET @number := 1;
SET @divisor := 1;
SELECT GROUP_CONCAT(n SEPARATOR '&')
FROM (SELECT @number := @number + 1 AS n 
          FROM information_schema.tables AS t1, information_schema.tables AS t2
         LIMIT 1000
       ) AS n1
WHERE NOT EXISTS(SELECT *
                    FROM (SELECT @divisor := @divisor + 1 AS d
                            FROM information_schema.tables AS t3, information_schema.tables AS t4
                           LIMIT 1000
                         ) AS n2
                   WHERE MOD(n, d) = 0 AND n <> d)
```

**Draw The Triangle 1**
```sql
* * * * * 
* * * * 
* * * 
* * 
*
```

```sql
SET @count = 21;
SELECT REPEAT('* ', @count := @count - 1)
FROM information_schema.tables 
WHERE @count > 0;
```

**Draw The Triangle 2**

```sql
* 
* * 
* * * 
* * * * 
* * * * *
```

```sql
SET @count = 0;
SELECT REPEAT('* ', @count := @count + 1)
FROM information_schema.tables 
WHERE @count < 20;
```
