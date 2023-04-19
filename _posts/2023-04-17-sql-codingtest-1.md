---
title: "[SQL HakerRank] Easy"
categories:
  - sql
  - haker rank
  - coding test
tags:
  - sql
  - haker rank 
  - coding test
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

<img width="377" alt="스크린샷 2023-04-04 17 11 24" src="https://user-images.githubusercontent.com/86525868/232437808-8bba0de0-0396-47a3-8739-8c5d47ebbdf8.png">{: width="40%" height="40%"}{: .align-center}

##### Q1. 

Query all columns for all American cities in the **CITY** table with populations larger than `100000`. The **CountryCode** for America is `USA`.

```sql
SELECT * FROM CITY WHERE POPULATION > 100000
```

##### Q2. 

Query the **NAME** field for all American cities in the **CITY** table with populations larger than `120000`. The *CountryCode* for America is `USA`.

```sql
SELECT NAME FROM CITY WHERE POPULATION > 12000 AND COUNTRYCODE = 'USA'
```

<img width="320" alt="image-20230404171936074" src="https://user-images.githubusercontent.com/86525868/232437824-a65d857b-04f1-47b6-aebd-63b0349cc439.png">{: width="40%" height="40%"}{: .align-center}

##### Q3. 

Query a list of **CITY** names from **STATION** for cities that have an even **ID** number. Print the results in any order, but exclude duplicates from the answer.

``` sql
SELECT DISTINCT CITY FROM STATION WHERE ID % 2 = 0
SELECT DISTINCT CITY FROM STATION WHERE MOD(ID, 2) = 0
```

##### Q4. 

Find the difference between the total number of **CITY** entries in the table and the number of distinct **CITY** entries in the table.

```sql
SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION
```

##### Q5. 

Query the two cities in **STATION** with the shortest and longest *CITY* names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

```sql
(SELECT CITY, LENGTH(CITY) 
	FROM STATION
	WHERE LENGTH(CITY) = (SELECT MIN(LENGTH(CITY)) FROM STATION)
 	ORDER BY CITY 
 	LIMIT 1)
 	
UNION 

(SELECT CITY, LENGTH(CITY)
	FROM STATION
	WHERE LENGTH(CITY) = (SELECT MAX(LENGTH(CITY)) FROM STATION)
	ORDER BY CITY 
	LIMIT 1)
```

##### Weather Observation Station 6

Query the list of *CITY* names starting with vowels (i.e., `a`, `e`, `i`, `o`, or `u`) from **STATION**. Your result *cannot* contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION 
	WHERE LEFT(CITY, 1) IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 7

Query the list of *CITY* names ending with vowels (a, e, i, o, u) from **STATION**. Your result *cannot* contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE RIGHT(CITY, 1) IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 8

Query the list of *CITY* names from **STATION** which have vowels (i.e., *a*, *e*, *i*, *o*, and *u*) as both their first *and* last characters. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE LEFT(CITY, 1) IN ('A', 'E', 'I', 'O', 'U') AND RIGHT(CITY, 1) IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 9

Query the list of *CITY* names from **STATION** that *do not start* with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE LEFT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 10

Query the list of *CITY* names from **STATION** that *do not end* with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE RIGHT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 11

Query the list of *CITY* names from **STATION** that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE LEFT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U') OR RIGHT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U')
```

##### Weather Observation Station 12

Query the list of *CITY* names from **STATION** that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY 
    FROM STATION 
    WHERE LEFT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U') AND RIGHT(CITY, 1) NOT IN ('A', 'E', 'I', 'O', 'U')
```

##### Higher than 75 Marks

Query the *Name* of any student in **STUDENTS** who scored higher than *Marks*. Order your output by the *last three characters* of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending *ID*.

```sql
SELECT NAME 
    FROM STUDENTS 
    WHERE MARKS > 75
    ORDER BY RIGHT(NAME, 3), ID ASC
```

<img width="379" alt="스크린샷 2023-04-05 10 25 25" src="https://user-images.githubusercontent.com/86525868/232437818-97c0043b-e995-4529-92e7-71eb8d1c4bc2.png">{: width="40%" height="40%"}{: .align-center}

##### Revising Aggregations

Query the total population of all cities in **CITY** where *District* is **California**.

```sql
SELECT SUM(POPULATION)
    FROM CITY 
    GROUP BY DISTRICT 
    HAVING DISTRICT = 'California'
```

##### Population Density Difference

Query the difference between the maximum and minimum populations in **CITY**.

```sql
SELECT MAX(POPULATION) - MIN(POPULATION)
    FROM CITY 
```

##### The Blunder

Samantha was tasked with calculating the average monthly salaries for all employees in the **EMPLOYEES** table, but did not realize her keyboard's key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: $actual - miscalculated$ average monthly salaries), and round it up to the next integer.

```sql
SELECT CEIL(AVG(SALARY) - AVG(REPLACE(CAST(SALARY AS CHAR),'0' , '')))
    FROM EMPLOYEES
```

> SQL Format Number Options
>
> `CAST` : 자료형 변환 `CAST(1234.12 AS INT)` 
>
> `CONVERT` : 자료형 변환 `CONVERT(INT, 1234.12)`
>
> `ROUND` : 반올림`ROUND(1234.1234, 2)` → `1234.1200`
>
> `CEILING` : 정수값으로 올림 `CEILING(1234.12)` → `1234`
>
> `FLOOR` : 소수점 아래 값 버림 `FLOOR(1234.1234)` → `1234`

##### Top Earners

We define an employee's *total earnings* to be their monthly $salary \times months$ worked, and the *maximum total earnings* to be the maximum total earnings for any employee in the **Employee** table. Write a query to find the *maximum total earnings* for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.

```sql
SELECT MONTHS*SALARY AS EARNINGS, COUNT(*)
    FROM EMPLOYEE
    WHERE MONTHS*SALARY = (SELECT MAX(MONTHS*SALARY) FROM EMPLOYEE)
    GROUP BY EARNINGS
```

##### Weather Observation Station 2

Query the following two values from the **STATION** table:

1. The sum of all values in *LAT_N* rounded to a scale of 2 decimal places.
2. The sum of all values in *LONG_W* rounded to a scale of 2 decimal places.

```sql
SELECT ROUND(SUM(LAT_N), 2)AS LAT, ROUND(SUM(LONG_W), 2)AS LON
    FROM STATION
```

##### Weather Observation Station 13

Query the sum of *Northern Latitudes* (*LAT_N*) from **STATION** having values greater than $38.7880$ and less than $137.2345$ . Truncate your answer to decimal places.

```sql
SELECT TRUNCATE(SUM(LAT_N), 4)
    FROM STATION 
    WHERE LAT_N BETWEEN 38.7880 AND 137.2345
```

##### Weather Observation Station 14

Query the greatest value of the *Northern Latitudes* (*LAT_N*) from **STATION** that is less than $137.2345$ . Truncate your answer to 4 decimal places.

```sql
SELECT TRUNCATE(MAX(LAT_N), 4)
    FROM STATION 
    WHERE LAT_N < 137.2345
```

##### Weather Observation Station 15

Query the *Western Longitude* (*LONG_W*) for the largest *Northern Latitude* (*LAT_N*) in **STATION** that is less than $137.2345$. Round your answer to 4 decimal places.

```sql
SELECT ROUND(LONG_W, 4)
    FROM STATION
    WHERE LAT_N < 137.2345 AND LAT_N = (SELECT MAX(LAT_N)
                                            FROM STATION
                                            WHERE LAT_N < 137.2345)
                                            
SELECT ROUND(LONG_W, 4)
	FROM STATION 
	WHERE LAT_N < 137.2345
	ORDER BY LAT_N DESC
	LIMIT 1
```

##### Weather Observation Station 16

Query the smallest *Northern Latitude* (*LAT_N*) from **STATION** that is greater than $38.7780$. Round your answer to 1 decimal places.

```sql
SELECT ROUND(LAT_N, 4)
    FROM STATION 
    WHERE LAT_N > 38.7780
    ORDER BY LAT_N ASC
    LIMIT 1	
```

#####  Weather Observation Station 17

Query the *Western Longitude* (*LONG_W*)where the smallest *Northern Latitude* (*LAT_N*) in **STATION** is greater than $38.7780$. Round your answer to 1 decimal places.

```sql
SELECT ROUND(LONG_W, 4)
    FROM STATION 
    WHERE LAT_N > 38.7780
    ORDER BY LAT_N
    LIMIT 1	
```

##### Weather Observation Station 18

Consider $P_1(a, b)$ and $P_2(c, d)$ to be two points on a *2D* plane.

- $a$ happens to equal the minimum value in *Northern Latitude* (*LAT_N* in **STATION**).
- $b$ happens to equal the minimum value in *Western Longitude* (*LONG_W* in **STATION**).
- $c$ happens to equal the maximum value in *Northern Latitude* (*LAT_N* in **STATION**).
- $d$ happens to equal the maximum value in *Western Longitude* (*LONG_W* in **STATION**).

Query the [Manhattan Distance](https://xlinux.nist.gov/dads/HTML/manhattanDistance.html) between points $P_1$ and $P_2$and round it to a scale of decimal places.

```sql
SELECT ROUND(ABS(MAX(LAT_N)-MIN(LAT_N))+ABS(MAX(LONG_W)-MIN(LONG_W)), 4)
    FROM STATION 
```

##### Weather Observation Station 19

Consider $P_1(a, c)$ and $P_2(b, d)$ to be two points on a 2D plane where $a, b$ are the respective minimum and maximum values of *Northern Latitude* (*LAT_N*) and $c, d$ are the respective minimum and maximum values of *Western Longitude* (*LONG_W*) in **STATION**.

Query the [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance) between points and and *format your answer* to display decimal digits.

```sql
SELECT ROUND(SQRT(POWER((MAX(LAT_N)-MIN(LAT_N)), 2)+POWER((MAX(LONG_W)-MIN(LONG_W)), 2)), 4)
    FROM STATION 
```

##### Weather Observation Station 20

A *[median](https://en.wikipedia.org/wiki/Median)* is defined as a number separating the higher half of a data set from the lower half. Query the *median* of the *Northern Latitudes* (*LAT_N*) from **STATION** and round your answer to decimal places.

```sql
# CASE 1
WITH TEMP AS (SELECT PERCENT_RANK() OVER (ORDER BY LAT_N) AS RNK, LAT_N
              	FROM STATION)
SELECT ROUND(AVG(LAT_N), 4)
	FROM TEMP
	WHERE RNK=0.5

# CASE 2
SELECT ROUND(LAT_N,4)
	FROM (SELECT LAT_N, PERCENT_RANK() OVER (ORDER BY LAT_N ASC) percent
      		FROM STATION) A
	WHERE percent=0.5

# CASE 3
SELECT ROUND(LAT_N,4)
    FROM STATION AS S
    WHERE ( SELECT COUNT(*) 
                FROM STATION 
                WHERE LAT_N < S.LAT_N) = (SELECT COUNT(*)
                                            FROM STATION 
                                            WHERE LAT_N > S.LAT_N)
```

##### Population Census

Given the **CITY** and **COUNTRY** tables, query the sum of the populations of all cities where the *CONTINENT* is *'Asia'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

```sql
SELECT SUM(C1.POPULATION)
    FROM CITY C1 INNER JOIN COUNTRY C2 ON C1.COUNTRYCODE = C2.CODE 
    WHERE C2.CONTINENT LIKE '%Asia%'
    
SELECT SUM(C1.POPULATION)   
    FROM CITY C1, COUNTRY C2 
    WHERE C1.COUNTRYCODE = C2.CODE AND C2.CONTINENT LIKE '%Asia%'
```

##### Average Population of Each Continent

Given the **CITY** and **COUNTRY** tables, query the names of all the continents (*COUNTRY.Continent*) and their respective average city populations (*CITY.Population*) rounded *down* to the nearest integer.

```sql
SELECT C2.CONTINENT, FLOOR(AVG(C1.POPULATION))
    FROM CITY C1, COUNTRY C2
    WHERE C1.COUNTRYCODE = C2.CODE 
    GROUP BY C2.CONTINENT
```

##### The Report 

*Ketty* gives *Eve* a task to generate a report containing three columns: *Name*, *Grade* and *Mark*. *Ketty* doesn't want the NAMES of those students who received a grade lower than *8*. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

```sql
SELECT IF(G.GRADE < 8, NULL, S.NAME), G.GRADE, S.MARKS 
    FROM STUDENTS S, GRADES G
    WHERE S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK
    ORDER BY G.GRADE DESC, S.NAME , S.MARKS 
```

##### Top Competitors 

