---
layout: post
author: ledinhtrunghieu
title: Note about Database and SQL
---

```sql
 LAG(quantity, 1) OVER() AS pre_quantity?

 WITH type_count AS (
	SELECT type, count(id) AS bottle_count
	FROM wine
	GROUP BY type
)

SELECT max(bottle_count) 
FROM 
FROM type_count

  substring(code, 3, 4) AS code_part


SELECT 
    AVG(energy) AS avg_energy,
    (SELECT AVG(price) FROM wine) AS avg_price
FROM food
```


Math.Round computes the nearest number to the input to a specified degree of accuracy.

Rounds a value to the nearest integer or to the specified number of fractional digits.

Math.Truncate effectively discards the any digits after the decimal point. It will always round to the nearest integer toward zero.