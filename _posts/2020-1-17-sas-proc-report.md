---
title: "SAS: Report to Excel"
layout: post
date: 2020-01-17 22:13
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview
**Output**:

- [ODS: TAGSETS.EXCELXP VS. EXCEL](#ODS) 
- [Proc Report](#proc-report)


---
## ODS
ODS (Output Delivery System): delivers output in a variety of formats, including HTML, Rich Text Format (RTF), PostScript (PS), Portable Document Format (PDF), and SAS data sets.
> Traditional SAS output: designed for line–printer

Basic ODS syntax:

```SAS
ODS TRACE ON </<options>>; 
ODS destination <FILE=filename>; 
ODS OUTPUT output-object-name=SAS-data-set-name; 
ODS <destination>SELECT output-object-name|ALL|NONE; 
...SAS procedure syntax... 
ODS <destination> CLOSE;  ODS TRACE OFF; 
```

- **Destinations**: ODS can be used to route quality presentation files to various destinations, including LISTING (default), HTML, RTF, PRINTER, and PDF.
- **Object**: Output objects are created by ODS to store the formatted results of most SAS procedures.
- **Style**: Styles define the presentation attributes of a report, such as font and color.

Very often, we report SAS tables to Excel. In this case we can utilize **ODS TAGSETS.EXCELXP** and **ODS EXCEL**.

### 1. ODS Excel

### 2. ODS TAGSETS.EXCELXP 

## Proc Report
To control the report


---
## Reference
- [Introduction to the Output Delivery System (ODS)](https://support.sas.com/content/dam/SAS/support/en/books/quick-results-with-the-output-delivery-system/58458_excerpt.pdf)
- [ODS TAGSETS.EXCELXP and ODS EXCEL SHOWDOWN](https://support.sas.com/resources/papers/proceedings17/0973-2017.pdf)
- [Unleash the Power of PROC REPORT
with the ODS EXCEL Destination](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2018/2479-2018.pdf)
