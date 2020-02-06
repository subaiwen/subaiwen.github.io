---
title: "SAS: Concatenating Data Sets"
layout: post
date: 2020-02-06 17:19
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview
- [Combining SAS Data Sets Vertically](#Combining-SAS-Data-Sets-Vertically)
- [Combining SAS Data Sets Horizontally](#Combining-SAS-Data-Sets-Horizontally)

---

## Combining SAS Data Sets Vertically
### 1. PROC APPEND
**Syntax:**

```SAS
PROC APPEND BASE=base-data-set DATA=append-data-set <FORCE>;
RUN;
```

- In `PROC APPEND` step, SAS does not read the observations in the `BASE`. Also, SAS cannot change any variable information in the [descriptor portion](https://www.sas.com/content/dam/SAS/en_ca/User%20Group%20Presentations/TASS/Capan-Descriptor.pdf) of the `BASE`
- The `FORCE` options causes SAS to drop the extra variables in `DATA`, and then to issue a warning message to the log.

### 2. SET
**Syntax:**

```SAS
DATA data-set;
	SET 
	data-set1(rename = (old-name-1 = new-name-1 
							old-name-2 = new-name-2
							...
							old-name-n = new-name-n)) 
	data-set2 
	...;
	<additional SAS statements> 
RUN;
```

- Because the DATA step creates a new data set, the input data sets can contain different variables. 
- If the data sets specified in the `SET` statement have a variable with the same name but different types, SAS generates a compile-time error by default.
- You can add additional `DATA step statements`, such as an assignment statement to create new variables in the output data set.

---

## Combining SAS Data Sets Horizontally
### 1. SET
**Syntax:**

```SAS
DATA data-set;
	SET data-set1;
	SET data-set2;
	... 
	SET data-setn;
RUN;
```

### 2. PROC SQL
**Syntax:**

```SAS
PROC SQL;
  CREATE TABLE table AS
  SELECT *
  FROM table-1
  CROSS JOIN table-2;
QUIT;
```
or

```SAS
PROC SQL;
  CREATE TABLE table AS
  SELECT *
  FROM table-1, table-2;
QUIT;
```

### 3. [MERGE](https://documentation.sas.com/?docsetId=lestmtsref&docsetTarget=n1i8w2bwu1fn5kn1gpxj18xttbb0.htm&docsetVersion=9.4&locale=en)
**Syntax:**

```SAS
DATA data-set;
	MERGE data-set1 data-set2 ... ; 
RUN;
```
`MERGE` statement would keep all the observations from the data sets. In other words, the number of obs. of final data set will be equal to the maximum number of obs. of the merging data sets.  
If you instead use multiple `SET` statements, the final data set will have minimun number of obs.

**MERGE By:**
`BY` in `Merge` statement is similar to `JOIN` in `PROC SQL`: 

```SAS
DATA data-set;
	MERGE data-set1 data-set2 ... ; 
	BY <DESCENDING> BY-variable(s);
	<additional SAS statements> 
RUN;
```
The `BY-variables` will be the bind keys.

**IN=:**

```SAS
DATA data-set;
	MERGE data-set1(IN=x) data-set2(IN=y) ... ; 
	BY <DESCENDING> BY-variable(s);
	<additional SAS statements> 
RUN;
```
Two new column `x` and `y` will be generated. `x` will have output `1` if the `By-variable` exists in `data-set1`, `0` otherwise. And the same for `y`.



---
## Reference
- [So You Think You Can Combine Data Sets?](https://support.sas.com/resources/papers/proceedings17/1526-2017.pdf)
- [Merge with Caution: How to Avoid Common Problems when Combining SAS Datasets](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2018/1746-2018.pdf)
- [Merging 3 datasets with different data formats](https://communities.sas.com/t5/SAS-Programming/Merging-3-datasets-with-different-data-formats/td-p/451813)
- [Introducing Data Relationships, Techniques for Data Manipulation, and Access Methods](https://www.sas.com/storefront/aux/en/spcombinmodify/60561_excerpt.pdf)
