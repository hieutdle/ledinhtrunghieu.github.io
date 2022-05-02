---
layout: post
author: ledinhtrunghieu
title: Test Preparation for Rackspace
---

# 1. Numerical Reasoning

**How old is John if, in 38 years, he will be 3 times as old as he is today?**
(38+x)=3x => 2x = 38 => x = 19

**The charts below show the number of cars John sold last year and the profit per car he made.**
bar chart
sum of  (profit per car sold * number of cars sold) of a type with three types
26000 + 12000 + 56000 = 94000


**The chart below shows how many properties an employee sold last month: What was the total number of properties sold last month?**

Notice 1 is confused with 0

**John has some apples, and two brothers, Jack and Jim. John eats 3 of his apples and splits the remainder equally between himself and each one of his brothers. Jack and Jim both eat half the apples they have been given. Together the three brothers have 4 apples left. How many apples did John start with?**

x - 3 
y + y/2 + y/2 = 4
=> y = 2
=> 3y = 6
=> 3y + 3 = x = 9

**It takes Billy 4 hours to paint a fence by himself. It takes Suzy 6 hours to paint the same fence by herself. How long will it take them if they start working on the same fence together and neither of them stops until the fence is completely painted?**

1 hours : 1/6 + 1/4 = 5/12
=> Finish 12/5 = 2.4h = 2 hours 24 minutes

**A brother and sister own equal parts in a company. The minority shareholders have the remaining 12,000 shares or 30% of the company. What is the number of shares that the sister owns?**

12000 shares = 30% => 1% = 400 shares * 100 = 40000 shares *35/100 = 14000 shares

**The charts below show the number of cars John sold last year and the profit he made per car. Considering all car types, in which quarter did John make the largest average profit per car?**

Q1 = profit / 3 = 6

Q2 = because Q2 sale 0 type => /2 => highest = 14

Q3 = /3 = high

Q4 = /3 = high

=> Q2


# 2. Verbal communication


**Your supervisor tells you that one of your coworkers, Dan, does not want to team up with you on the next project because something you said hurt his feelings. How do you address the situation?**

Apologize to your supervisor and promise it won't happen again.
Make a point of being extra friendly to all of your coworkers, including Dan, going forward.
**Meet privately with Dan to find out how you hurt his feelings and apologize for doing so.**
Ask your supervisor what you said that hurt Dan’s feelings and avoid doing that in the future.
Send Dan an email apologizing for hurting his feelings and CC your supervisor.

**You have been tasked with giving a presentation at work. Due to the level of detail required, the presentation is scheduled for a total of three hours. About an hour into the presentation, you notice that the audience is getting distracted and starting to pay more attention to their phones than to you. Which methods would be effective at re-engaging your audience?**

Pass out some supplementary reading material and give them time to read through it.
Give the audience a single point of focus by remaining very still, refraining from moving around or gesturing.
Speed up the presentation by reducing your remarks to solely reading each slide's text out loud to the audience.
**Ask the audience to answer a series of yes/no questions by raising their hands for 'yes'.**
**Invite the audience to ask questions on any of the material covered so far.**

# 3. SQL

**A table containing the students enrolled in a yearly course has incorrect data in records with ids between 20 and 100 (inclusive).**

```sql
TABLE enrollments
  id INTEGER NOT NULL PRIMARY KEY
  year INTEGER NOT NULL
  studentId INTEGER NOT NULL
```

**Write a query that updates the field 'year' of every faulty record to 2015.**

```sql
UPDATE enrollments
SET year = 2015
WHERE id BETWEEN 20 AND 100
```


**Information about pets is kept in two separate tables:**

```sql
TABLE dogs
  id INTEGER NOT NULL PRIMARY KEY,
  name VARCHAR(50) NOT NULL

TABLE cats
  id INTEGER NOT NULL PRIMARY KEY,
  name VARCHAR(50) NOT NULL
```

**Write a query that select all distinct pet names.**

```sql
SELECT distinct name
FROM dogs
UNION
SELECT distinct name
FROM cats
```

**App usage data are kept in the following table:**

```sql
TABLE sessions
  id INTEGER PRIMARY KEY,
  userId INTEGER NOT NULL,
  duration DECIMAL NOT NULL
```
**Write a query that selects userId and average session duration for each user who has more than one session.**
```sql
SELECT userid, AVG(duration)
FROM sessions
GROUP BY userid
HAVING COUNT(userid) > 1
```

