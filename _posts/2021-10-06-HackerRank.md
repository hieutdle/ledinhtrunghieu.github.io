---
layout: post
author: ledinhtrunghieu
title: HackerRank
---

# 1. Python

**sWAP cASE**
```python
def swap_case(s):
    s = s.swapcase()
    return s
```




# 2. SQL

**The PADS**

```sql
SELECT CONCAT(NAME,'(',SUBSTR(OCCUPATION,1,1),')') AS N
FROM OCCUPATIONS
ORDER BY N;

SELECT CONCAT('There are a total of ',COUNT(OCCUPATION),' ',LOWER(OCCUPATION),'s.')
FROM OCCUPATIONS
GROUP BY OCCUPATION
ORDER BY COUNT(OCCUPATION), OCCUPATION;
```

**Type of Triangle**
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


# 3. ALgorithms



# 4. Data Structures



