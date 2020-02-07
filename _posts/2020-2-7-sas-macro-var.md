---
title: "SAS: Macro Variable"
layout: post
date: 2020-02-07 01:22
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview
- [%LET statement](#LET-statement)
- [Using the INTO in PROC SQL](#Using-the-INTO-in-PROC-SQL)
- [Using the CALL SYMPUTX routine](#Using-the-CALL-SYMPUTX-routine)

--

## 1. %LET statement
**Syntax:**

```SAS
%LET macro-variable-name = text-or-text-value;
```

## 2. Using the INTO in PROC SQL
**Creating a single value:**

```SAS
PROC SQL;
	SELECT var-1, var-2
	INTO :macro-var-1, :macro-var-2
	FROM table;
QUIT;
```

Note: only the first record is assigned to the macro variable, when there are multiple records.

**Creating lists of values:**

Using `SEPERATED BY`:

```SAS
PROC SQL;
	SELECT var-1, var-2
	INTO :macro-var-1 SEPERATED BY ',', 
		 :macro-var-2 SEPERATED BY ' '
	FROM table;
QUIT;
```

Using `THROUGH`:

```SAS
PROC SQL;
	SELECT var-1, var-2
	INTO :macro-var-1-x THROUGH :macro-var-1-y, 
		 :macro-var-2-x THROUGH :macro-var-2-y
	FROM table;
QUIT;
```

From `&macro-var-1-x` to `macro-var-1-y` variables, each of them stores each record of `var-1` from the $x^{th}$ row to the $y^{th}$ row. And the same for `&macro-var-2-x` to `macro-var-2-y`.

> No upper bound:
> 
```SAS
PROC SQL;
	SELECT var-1, var-2
	INTO :macro-var-1-, 
		 :macro-var-2-
	FROM table;
QUIT;
```

## 3. Using the CALL SYMPUTX routine
**Syntax:**

```SAS
DATA _NULL_;
	CALL SYMPUT(macro_var, value);
RUN;
```


---
## Reference
- [Five Ways to Create Macro Variables:
A Short Introduction to the Macro Language](https://support.sas.com/resources/papers/proceedings17/1516-2017.pdf)
- [proc sql - select into](https://renenyffenegger.ch/notes/Companies-Products/SAS/programming/proc/sql/select/into/index)
- [SYMPUT and SYMGET: Getting DATA Step Variables and Macro Variables to Share](https://www.lexjansen.com/nesug/nesug04/pm/pm13.pdf)

