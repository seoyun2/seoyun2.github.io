---
title: "[SQL HakerRank] Easy"
categories:
  - sql
  - haker rank
  - coding test
tags:
  - sql
  - haker rank 
  coding test
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

![](2023-04-17-18-01-29.png)

##### Q1. 

Query all columns for all American cities in the **CITY** table with populations larger than `100000`. The **CountryCode** for America is `USA`.

```SQL 
SELECT * FROM CITY WHERE POPULATION > 100000
```

##### Q2. 

Query the **NAME** field for all American cities in the **CITY** table with populations larger than `120000`. The *CountryCode* for America is `USA`.

```SQL
SELECT NAME FROM CITY WHERE POPULATION > 12000 AND COUNTRYCODE = 'USA'
```

